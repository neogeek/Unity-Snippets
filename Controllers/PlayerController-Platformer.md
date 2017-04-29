# PlayerController.cs (Platformer)

### Variables

- **groundLayers** - Layer mask with only the platforms and floors selected.
- **groundTransform** - An empty game object positioned at the bottom of the player object. Must be a child of the player object.

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(Rigidbody2D))]
[RequireComponent(typeof(SpriteRenderer))]
public class PlayerController : MonoBehaviour {

    public LayerMask groundLayers;
    public Transform groundTransform;

    private Rigidbody2D rb;
    private Animator anima;

    private float horizontalSpeed = 10.0f;
    private float jumpForce = 700.0f;

    private bool jump = false;
    private bool grounded = false;

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

        grounded = Physics2D.OverlapCircle(groundTransform.position, 0.5f, groundLayers);

        Vector2 movement = new Vector2(moveHorizontal * horizontalSpeed, rb.velocity.y);

        rb.velocity = movement;

        if (grounded && jump) {

            rb.AddForce(new Vector2(0, jumpForce));

        }

    }

    void Flip (float moveHorizontal) {

        Vector2 scale = gameObject.transform.localScale;
        scale.x = Mathf.Abs(scale.x) * Mathf.Sign(moveHorizontal);

        gameObject.transform.localScale = scale;

    }

}
```
