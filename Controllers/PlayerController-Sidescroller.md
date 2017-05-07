# PlayerController.cs (Sidescroller)

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(SpriteRenderer))]
public class PlayerController : MonoBehaviour {

    private Animator anima;

    public Transform punchTrigger;

    private float horizontalSpeed = 8.0f;
    private float verticalSpeed = 5.0f;

    void Start () {

        anima = gameObject.GetComponent<Animator>();

    }

    void FixedUpdate () {

        float moveHorizontal = Input.GetAxisRaw("Horizontal");
        float moveVertical = Input.GetAxisRaw("Vertical");

        bool punch = Input.GetButton("Fire1");

        anima.SetBool("punching", punch);

        bool isPunching = anima.GetCurrentAnimatorStateInfo(0).IsName("Punch");

        if (!isPunching) {

            anima.SetBool("running", (Mathf.Abs(moveHorizontal) > 0 || Mathf.Abs(moveVertical) > 0));

            if (Mathf.Abs(moveHorizontal) > 0) {

                Flip(moveHorizontal);

            }

            transform.Translate(new Vector2(
                moveHorizontal * horizontalSpeed * Time.deltaTime,
                moveVertical * verticalSpeed * Time.deltaTime
            ));

        } else {

            Collider2D[] punched = Physics2D.OverlapCircleAll(punchTrigger.position, 0.5f, 1 << LayerMask.NameToLayer("Enemy"));

            for (int i = 0; i < punched.Length; i++) {

                punched[i].GetComponent<Animator>().SetBool("punched", true);

            }

        }

    }

    void Flip (float horizontalDirection) {

        Vector2 scale = gameObject.transform.localScale;
        scale.x = Mathf.Abs(scale.x) * Mathf.Sign(horizontalDirection);

        gameObject.transform.localScale = scale;

    }

}
```
