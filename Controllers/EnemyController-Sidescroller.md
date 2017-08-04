# EnemyController.cs (Sidescroller)

```csharp
using UnityEngine;

[RequireComponent(typeof (Animator))]
[RequireComponent(typeof (SpriteRenderer))]
public class EnemyController : MonoBehaviour {

    public GameObject player;
    public Transform moveTarget;

    private Animator anima;
    private SpriteRenderer sprite;
    private Transform playerMoveTarget;

    private readonly float stunnedDelayRate = 0.5f;
    private readonly float maxSpeed = 5.0f;
    private readonly float maxDistanceX = 3.0f;
    private readonly float maxDistanceY = 1.0f;

    private float stunnedNextTick = 0f;
    private Vector3 distanceToPlayer = Vector3.zero;
    private int horizontalDirection = 1;
    private bool hasBeenPunched = false;

    void Awake() {

        anima = gameObject.GetComponent<Animator>();
        sprite = gameObject.GetComponent<SpriteRenderer>();

        playerMoveTarget = player.GetComponent<PlayerController>().moveTarget;

    }

    void FixedUpdate() {

        distanceToPlayer = playerMoveTarget.position - moveTarget.position;

        if (hasBeenPunched) {

            Punched();

        } else {

            if (Mathf.Abs(distanceToPlayer.x) > 0 && Mathf.Sign(distanceToPlayer.x) != horizontalDirection) {

                Flip();

            }

            Move();

        }

        UpdateSortingOrder();

    }

    void Move() {

        if (Mathf.Abs(distanceToPlayer.x) > maxDistanceX || Mathf.Abs(distanceToPlayer.y) > maxDistanceY) {

            transform.position = Vector3.MoveTowards(moveTarget.position, playerMoveTarget.position, maxSpeed * Time.deltaTime);

            anima.SetBool("walking", true);

        } else {

            anima.SetBool("walking", false);

        }

    }

    void Flip() {

        Vector2 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

    }

    void UpdateSortingOrder() {

        if (moveTarget.position.y > playerMoveTarget.position.y) {

            sprite.sortingOrder = -1;

        } else {

            sprite.sortingOrder = 1;

        }

    }

    void Punched() {

        if (Time.time > stunnedNextTick) {

            hasBeenPunched = false;

            stunnedNextTick = 0f;

        }

        anima.SetBool("punched", hasBeenPunched);

    }

    public void Punch() {

        hasBeenPunched = true;

        stunnedNextTick = Time.time + stunnedDelayRate;

    }

}
```
