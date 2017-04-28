# PlayerController.cs

### Variables

- **groundLayers** - Layer mask with only the platforms and floors selected.
- **groundTransform** - An empty game object positioned at the bottom of the player object. Must be a child of the player object.

```csharp
using UnityEngine;

[RequireComponent(typeof(Rigidbody2D))]
public class PlayerController : MonoBehaviour {

    public LayerMask groundLayers;
    public Transform groundTransform;

    private Rigidbody2D rb;

    private float moveSpeed = 10.0f;
    private float jumpForce = 700.0f;

    private bool jump = false;
    private bool grounded = false;

    void Start () {

        if (Physics2D.gravity.y != -30.0f) {

            Debug.LogWarning("PlayerController runs smoother when gravity is set to 0,-30.");

        }

        rb = gameObject.GetComponent<Rigidbody2D>();

    }

    void Update () {

        jump = Input.GetKeyDown(KeyCode.Space);

    }

    void FixedUpdate () {

        float moveHorizontal = Input.GetAxis("Horizontal");

        grounded = Physics2D.OverlapCircle(groundTransform.position, 0.5f, groundLayers);

        Vector2 movement = new Vector2(moveHorizontal * moveSpeed, rb.velocity.y);

        rb.velocity = movement;

        if (grounded && jump) {

            rb.AddForce(new Vector2(0, jumpForce));

        }

    }

}
```
