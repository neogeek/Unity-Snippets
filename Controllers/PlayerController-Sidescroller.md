# PlayerController.cs (Sidescroller)

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(Rigidbody2D))]
[RequireComponent(typeof(SpriteRenderer))]
public class PlayerController : MonoBehaviour {

    private Rigidbody2D rb;
    private Animator anima;

    private float horizontalSpeed = 10.0f;
    private float verticalSpeed = 10.0f;

    void Start () {

        if (Physics2D.gravity.y != -30.0f) {

            Debug.LogWarning("PlayerController runs smoother when gravity is set to 0,-30.");

        }

        anima = gameObject.GetComponent<Animator>();

        rb = gameObject.GetComponent<Rigidbody2D>();

    }

    void FixedUpdate () {

        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");

        anima.SetFloat("speed", Mathf.Abs(moveHorizontal));

        if (Mathf.Abs(moveHorizontal) > 0) {

            Flip(moveHorizontal);

        }

        Vector2 movement = new Vector2(moveHorizontal * horizontalSpeed, moveVertical * verticalSpeed);

        rb.velocity = movement;

    }

    void Flip (float moveHorizontal) {

        Vector2 scale = gameObject.transform.localScale;
        scale.x = Mathf.Abs(scale.x) * Mathf.Sign(moveHorizontal);

        gameObject.transform.localScale = scale;

    }

}
```
