# PlayerController.cs (Sidescroller)

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(SpriteRenderer))]
public class PlayerController : MonoBehaviour {

    private Animator anima;

	private SpriteRenderer sprite;

    private float horizontalSpeed = 8.0f;
    private float verticalSpeed = 5.0f;

    void Start () {

        anima = gameObject.GetComponent<Animator>();

		sprite = gameObject.GetComponent<SpriteRenderer>();

    }

    void FixedUpdate () {

        float moveHorizontal = Input.GetAxisRaw("Horizontal");
        float moveVertical = Input.GetAxisRaw("Vertical");

		bool punch = Input.GetButton("Fire1");

		anima.SetBool("punching", punch);

		if (!punch) {

			anima.SetBool("running", (Mathf.Abs(moveHorizontal) > 0 || Mathf.Abs(moveVertical) > 0));

			if (Mathf.Abs(moveHorizontal) > 0) {

				Flip(moveHorizontal);

			}

			transform.Translate(new Vector2(
				moveHorizontal * horizontalSpeed * Time.deltaTime,
				moveVertical * verticalSpeed * Time.deltaTime
			));

		}

    }

    void Flip (float moveHorizontal) {

        sprite.flipX = Mathf.Sign(moveHorizontal) < 0;

    }

}
```
