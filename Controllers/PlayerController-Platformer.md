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

    private bool jump = false;
    private bool grounded = false;
    private bool doubleJump = false;

    void Start () {

        if (Physics2D.gravity.y != -30.0f) {

            Debug.LogWarning("PlayerController runs smoother when gravity is set to 0,-30.");

        }

        anima = gameObject.GetComponent<Animator>();
        rb = gameObject.GetComponent<Rigidbody2D>();

    }

    void Update () {

        jump = Input.GetKeyDown(KeyCode.Space);

    }

    void FixedUpdate () {

        float moveHorizontal = Input.GetAxis("Horizontal");

        anima.SetFloat("speed", Mathf.Abs(moveHorizontal));

        if (Mathf.Abs(moveHorizontal) > 0) {

            Flip(moveHorizontal);

        }

        grounded = Physics2D.OverlapCircle(groundTrigger.position, 0.5f, groundLayers);

        if (grounded) {

            doubleJump = false;

        }

        Vector2 movement = new Vector2(moveHorizontal * horizontalSpeed, rb.velocity.y);

        rb.velocity = movement;

        if ((grounded || !doubleJump) && jump) {

            rb.velocity = new Vector2(rb.velocity.x, 0);

            rb.AddForce(new Vector2(0, jumpForce));

            if (!grounded) {

                doubleJump = true;

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
