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

    private float stunnedDelayRate = 0.25f;
    private float stunnedNextTick = 0f;

    private float maxSpeed = 5.0f;
    private float maxDistanceX = 3.0f;
    private float maxDistanceY = 1.0f;

    void Start () {

        anima = gameObject.GetComponent<Animator>();
        sprite = gameObject.GetComponent<SpriteRenderer>();
        playerSprite = player.GetComponent<SpriteRenderer>();

    }

    void Update () {

        if (anima.GetBool("punched")) {

            if (stunnedNextTick == 0f) {

                stunnedNextTick = Time.time + stunnedDelayRate;

            }

            if (Time.time > stunnedNextTick) {

                anima.SetBool("punched", false);

                stunnedNextTick = 0f;

            }

        }

    }

    void FixedUpdate () {

        float distanceX = Mathf.Abs(sprite.bounds.min.x - playerSprite.bounds.min.x);
        float distanceY = Mathf.Abs(sprite.bounds.min.y - playerSprite.bounds.min.y);

        Flip(player.transform.position.x - transform.position.x);

        if (sprite.bounds.min.y > playerSprite.bounds.min.y) {

            sprite.sortingOrder = -1;

        } else {

            sprite.sortingOrder = 1;

        }

        if (player && player.activeSelf && (distanceX > maxDistanceX || distanceY > maxDistanceY)) {

            transform.position = Vector3.MoveTowards(sprite.bounds.min, playerSprite.bounds.min, maxSpeed * Time.deltaTime) + sprite.bounds.extents;

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
