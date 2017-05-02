# EnemyController.cs (Sidescroller)

```csharp
using UnityEngine;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(SpriteRenderer))]
public class EnemyController : MonoBehaviour {

    public GameObject player;

    private Animator anima;

    private SpriteRenderer sprite;
    private SpriteRenderer playerSprite;

    private float maxSpeed = 5.0f;
    private float maxDistance = 3.0f;

    void Start () {

        anima = gameObject.GetComponent<Animator>();
        sprite = gameObject.GetComponent<SpriteRenderer>();
        playerSprite = player.GetComponent<SpriteRenderer>();

    }

    void FixedUpdate () {

        float distance = Vector2.Distance(transform.position, player.transform.position);

        Flip(player.transform.position.x - transform.position.x);

        if (sprite.bounds.min.y > playerSprite.bounds.min.y) {

            sprite.sortingOrder = -1;

        } else {

            sprite.sortingOrder = 1;

        }

        if (player && player.activeSelf && distance > maxDistance) {

            transform.position = Vector2.MoveTowards(transform.position, player.transform.position, maxSpeed * Time.deltaTime);

            anima.SetBool("running", true);

        } else {

            anima.SetBool("running", false);

        }

    }

    void Flip (float horizontalDirection) {

        Vector2 scale = gameObject.transform.localScale;
        scale.x = Mathf.Abs(scale.x) * Mathf.Sign(horizontalDirection);

        gameObject.transform.localScale = scale;

    }

}
```
