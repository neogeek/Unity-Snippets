# PlayerController.cs (Sidescroller)

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(SpriteRenderer))]
public class PlayerController : MonoBehaviour {

    public Transform punchTrigger;
    public Transform moveTarget;
    public LayerMask enemyLayerMask;

    private Animator anima;
    private SpriteRenderer sprite;

    private readonly float horizontalSpeed = 8.0f;
    private readonly float verticalSpeed = 5.0f;

    private float moveHorizontal = 0.0f;
    private float moveVertical = 0.0f;
    private int horizontalDirection = 1;
    private bool punchPressed = false;

    void Awake () {

        anima = gameObject.GetComponent<Animator>();
        sprite = gameObject.GetComponent<SpriteRenderer>();

    }

    void Update () {

        moveHorizontal = Input.GetAxisRaw("Horizontal");
        moveVertical = Input.GetAxisRaw("Vertical");

        punchPressed = Input.GetButtonDown("Fire1");

    }

    void FixedUpdate () {

        if (punchPressed) {

            Punch();

        } else {

            if (Mathf.Abs(moveHorizontal) > 0 && Mathf.Sign(moveHorizontal) != horizontalDirection) {

                Flip();

            }

            Move();

        }

    }

    void Move () {

        anima.SetFloat("speed", Mathf.Abs(moveHorizontal) + Mathf.Abs(moveVertical));

        transform.Translate(new Vector2(
            moveHorizontal * horizontalSpeed * Time.deltaTime,
            moveVertical * verticalSpeed * Time.deltaTime
        ));

    }

    void Flip () {

        Vector2 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

    }

    void Punch () {

        anima.SetTrigger("punching");

        Collider2D[] punched = Physics2D.OverlapCircleAll(punchTrigger.position, 0.5f, enemyLayerMask);

        for (int i = 0; i < punched.Length; i++) {

            punched[i].GetComponent<EnemyController>().Punch();

        }

    }

}
```
