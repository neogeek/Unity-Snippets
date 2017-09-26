# PlayerController.cs (Platformer)

```csharp
using UnityEngine;

[RequireComponent(typeof (BoxCollider2D))]
[RequireComponent(typeof (Rigidbody2D))]
public class PlayerController : MonoBehaviour {

    public Transform groundTrigger;
    public LayerMask groundLayers;

    public Transform wallTrigger;
    public LayerMask wallLayers;

    private Rigidbody2D rb;

    private readonly float horizontalSpeed = 10.0f;
    private readonly float jumpForce = 700.0f;
    private readonly int maxJumpCount = 2;
    private readonly float triggerRadius = 0.1f;
    private readonly float maxWallSlideSpeed = -1.0f;

    private bool grounded = false;
    private bool touchingWall = false;

    private float moveHorizontal = 0.0f;
    private int horizontalDirection = 1;
    private int currentJumpCount = 0;

    private bool _inputJump = false;
    private bool inputJump {
        get {
            return _inputJump;
        }
        set {

            if (value) {
                _inputJump = true;
            }

        }
    }

    void Awake() {

        rb = gameObject.GetComponent<Rigidbody2D>();

    }

    void Update() {

        moveHorizontal = Input.GetAxis("Horizontal");
        inputJump = Input.GetButtonDown("Jump");

    }

    void FixedUpdate() {

        grounded = Physics2D.OverlapCircle(groundTrigger.position, triggerRadius, groundLayers);
        touchingWall = Physics2D.OverlapCircle(wallTrigger.position, triggerRadius, wallLayers);

        Move();
        Jump();
        WallSlide();

    }

    void Move() {

        if (Mathf.Abs(moveHorizontal) > 0 && Mathf.Sign(moveHorizontal) != horizontalDirection) {

            Flip();

        }

        if (Mathf.Abs(moveHorizontal) > 0) {

            rb.velocity = new Vector2(moveHorizontal * horizontalSpeed, rb.velocity.y);

        }

    }

    void Jump() {

        if (grounded) {

            currentJumpCount = 0;

        }

        if (inputJump && currentJumpCount < maxJumpCount) {

            rb.velocity = new Vector2(rb.velocity.x, 0);

            rb.AddForce(new Vector2(0, jumpForce));

            currentJumpCount += 1;

        }

        if (inputJump) {

            _inputJump = false;

        }

    }

    void WallSlide() {

        if (touchingWall && !grounded && moveHorizontal != 0) {

            rb.velocity = new Vector2(rb.velocity.x, Mathf.Max(rb.velocity.y, maxWallSlideSpeed));

        }

    }

    void Flip() {

        Vector3 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

    }

    void OnDrawGizmos() {

        Gizmos.color = Color.green;
        Gizmos.DrawWireSphere(groundTrigger.position, triggerRadius);
        Gizmos.DrawWireSphere(wallTrigger.position, triggerRadius);

    }

}
```
