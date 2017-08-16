# PlayerController.cs (Platformer)

```csharp
using UnityEngine;

[RequireComponent(typeof (Rigidbody2D))]
public class PlayerController : MonoBehaviour {

    public Transform groundTrigger;
    public LayerMask groundLayers;

    private Rigidbody2D rb;

    private readonly float horizontalSpeed = 10.0f;
    private readonly float jumpForce = 700.0f;
    private readonly int maxJumpCount = 2;
    private readonly float triggerRadius = 0.1f;

    private float moveHorizontal = 0.0f;
    private int horizontalDirection = 1;
    private bool jumpPressed = false;
    private int currentJumpCount = 0;

    void Awake() {

        rb = gameObject.GetComponent<Rigidbody2D>();

    }

    void Update() {

        moveHorizontal = Input.GetAxis("Horizontal");
        jumpPressed = Input.GetKeyDown(KeyCode.Space) || (Input.touchCount > 0 && Input.GetTouch(0).phase == TouchPhase.Began);

    }

    void FixedUpdate() {

        if (Mathf.Abs(moveHorizontal) > 0 && Mathf.Sign(moveHorizontal) != horizontalDirection) {

            Flip();

        }

        Move();
        Jump();

    }

    void Move() {

        rb.velocity = new Vector2(moveHorizontal * horizontalSpeed, rb.velocity.y);

    }

    void Jump() {

        bool grounded = Physics2D.OverlapCircle(groundTrigger.position, triggerRadius, groundLayers);

        if (grounded) {

            currentJumpCount = 0;

        }

        if (jumpPressed && currentJumpCount < maxJumpCount) {

            rb.velocity = new Vector2(rb.velocity.x, 0);

            rb.AddForce(new Vector2(0, jumpForce));

            currentJumpCount += 1;

        }

    }

    void Flip() {

        Vector2 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

    }

    void OnDrawGizmos() {

        Gizmos.color = Color.green;
        Gizmos.DrawWireSphere(groundTrigger.position, triggerRadius);

    }

}
```
