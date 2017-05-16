# PlayerController.cs (Platformer)

### Variables

- **groundTrigger** - An empty game object positioned at the bottom of the player object. Must be a child of the player object.
- **groundLayers** - Layer mask with only the platforms and floors selected.

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(Rigidbody2D))]
[RequireComponent(typeof(SpriteRenderer))]
public class PlayerController : MonoBehaviour {

    public Transform groundTrigger;
    public LayerMask groundLayers;

    private Rigidbody2D rb;
    private Animator anima;

    private float horizontalSpeed = 10.0f;
    private float jumpForce = 700.0f;

    private bool jumpPressed = false;
    private bool canJump = true;
    private bool doubleJumpUsed = false;

    private float jumpDelayRate = 0.1f;
    private float nextJumpTick;

    void Start () {

        if (Physics2D.gravity.y != -30.0f) {

            Debug.LogWarning("PlayerController runs smoother when gravity is set to 0,-30.");

        }

        rb = gameObject.GetComponent<Rigidbody2D>();
        anima = gameObject.GetComponent<Animator>();

    }

    void Update () {

        jumpPressed = Input.GetKeyDown(KeyCode.Space) || (Input.touchCount > 0 && Input.GetTouch(0).phase == TouchPhase.Began);

    }

    void FixedUpdate () {

        float moveHorizontal = Input.GetAxis("Horizontal");

        anima.SetFloat("speed", Mathf.Abs(moveHorizontal));

        if (Mathf.Abs(moveHorizontal) > 0) {

            Flip(moveHorizontal);

        }

        Vector2 movement = new Vector2(moveHorizontal * horizontalSpeed, rb.velocity.y);

        rb.velocity = movement;

        if (Time.time > nextJumpTick) {

            bool grounded = Physics2D.OverlapCircle(groundTrigger.position, 0.1f, groundLayers);

            if (grounded) {

                canJump = true;
                doubleJumpUsed = false;

            }

            if (canJump && jumpPressed) {

                rb.velocity = new Vector2(rb.velocity.x, 0);

                rb.AddForce(new Vector2(0, jumpForce));

                if (!doubleJumpUsed) {

                    doubleJumpUsed = true;

                } else {

                    canJump = false;

                }

                nextJumpTick = Time.time + jumpDelayRate;

            }

        }

    }

    void Flip (float moveHorizontal) {

        Vector2 scale = gameObject.transform.localScale;
        scale.x = Mathf.Abs(scale.x) * Mathf.Sign(moveHorizontal);

        gameObject.transform.localScale = scale;

    }

}
```
