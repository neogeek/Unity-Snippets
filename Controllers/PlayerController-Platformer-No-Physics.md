# PlayerController.cs (Platformer w/ no Physics)

```csharp
using System;
using System.Collections;
using UnityEngine;

public class PlayerController : MonoBehaviour {

    public Transform platformTrigger;
    public LayerMask platformLayerMask;

    public Transform wallTrigger;
    public LayerMask wallLayerMask;

    public enum STATE {
        PLAYER_IDLE,
        PLAYER_RUNNING,
        PLAYER_FALLING,
        PLAYER_JUMPING,
        PLAYER_WALL_SLIDE,
        PLAYER_WALL_JUMP,
        PLAYER_WALL_DISMOUNT
    }

    [SerializeField]
    private STATE _state = STATE.PLAYER_IDLE;

    public STATE state {
        get {
            return _state;
        }
        set {

            Debug.Log(string.Format("Switched from {0} to {1}.", _state, value));

            _state = value;

        }
    }

    private readonly float gravity = 20.0f;
    private readonly float wallSlideSpeed = -2.0f;
    private readonly float horizontalSpeed = 6.0f;
    private readonly float horizontalInputThreshold = 1.0f;
    private readonly float jumpSpeed = 14.0f;
    private readonly int maxAvalibleJumps = 2;
    private readonly WaitForSeconds horizontalMovementDelay = new WaitForSeconds(0.5f);
    private readonly WaitForSeconds jumpDelay = new WaitForSeconds(0.5f);
    private IEnumerator jumpDelayCoroutine;

    private Vector2 velocity = Vector2.zero;
    private int horizontalDirection = 1;

    private float inputHorizontal = 0;
    private bool inputHorizontalEnabled = true;
    private bool inputJump = false;
    private int inputJumpsAvalible = 0;

    void Update() {

        if (inputHorizontalEnabled) {

            inputHorizontal = Input.GetAxisRaw("Horizontal");

        }

        inputJump = Input.GetKeyDown(KeyCode.Space);

    }

    void FixedUpdate() {

        switch (state) {

            case STATE.PLAYER_IDLE:
                Idle();
                break;

            case STATE.PLAYER_RUNNING:
                Running();
                break;

            case STATE.PLAYER_FALLING:
                Falling();
                break;

            case STATE.PLAYER_JUMPING:
                Jumping();
                break;

            case STATE.PLAYER_WALL_SLIDE:
                WallSlide();
                break;

            case STATE.PLAYER_WALL_JUMP:
                WallJump();
                break;

            case STATE.PLAYER_WALL_DISMOUNT:
                WallDismount();
                break;

            default:
                break;

        }

    }

    void Idle() {

        if (jumpDelayCoroutine != null) {

            StopCoroutine(jumpDelayCoroutine);

        }

        inputJumpsAvalible = maxAvalibleJumps;

        velocity = Vector2.zero;

        Vector2 platformPoint = GetNextPlatformPoint();
        Vector2 wallPoint = GetNextWallPoint();

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        if ((inputHorizontal == 1 && wallPoint.x > gameObject.transform.position.x) ||
            (inputHorizontal == -1 && wallPoint.x < gameObject.transform.position.x)) {

            state = STATE.PLAYER_RUNNING;

            return;

        }

        if (platformPoint.y < platformTrigger.position.y) {

            state = STATE.PLAYER_FALLING;

            return;

        }

        if (inputJump) {

            inputJumpsAvalible -= 1;

            state = STATE.PLAYER_JUMPING;

            return;

        }

    }

    void Running() {

        if (jumpDelayCoroutine != null) {

            StopCoroutine(jumpDelayCoroutine);

        }

        inputJumpsAvalible = maxAvalibleJumps;

        velocity.x = inputHorizontal * horizontalSpeed;
        velocity.y = 0;

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        Vector2 platformPoint = GetNextPlatformPoint();
        Vector2 wallPoint = GetNextWallPoint();

        gameObject.transform.position = CalculatePlayerMovement(gameObject.transform.position, platformPoint, wallPoint);

        if (gameObject.transform.position.x == wallPoint.x) {

            state = STATE.PLAYER_IDLE;

            return;

        }

        if (platformPoint.y < platformTrigger.position.y) {

            state = STATE.PLAYER_FALLING;

            return;

        }

        if (inputJump) {

            inputJumpsAvalible -= 1;

            state = STATE.PLAYER_JUMPING;

            return;

        }

        if (inputHorizontal == 0) {

            state = STATE.PLAYER_IDLE;

            return;

        }

    }

    void Falling() {

        if (inputJumpsAvalible > 0) {

            jumpDelayCoroutine = DisallowJump();

            StartCoroutine(jumpDelayCoroutine);

        }

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = inputHorizontal * horizontalSpeed;

        }

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        velocity.y -= gravity * Time.deltaTime;

        Vector2 platformPoint = GetNextPlatformPoint();
        Vector2 wallPoint = GetNextWallPoint();

        gameObject.transform.position = CalculatePlayerMovement(gameObject.transform.position, platformPoint, wallPoint);

        if (inputJumpsAvalible > 0 && inputJump) {

            inputJumpsAvalible -= 1;

            velocity.y = 0;

            state = STATE.PLAYER_JUMPING;

            return;

        }

        if (gameObject.transform.position.x == wallPoint.x) {

            state = STATE.PLAYER_WALL_SLIDE;

            return;

        }

        if (gameObject.transform.position.y == platformPoint.y) {

            state = STATE.PLAYER_RUNNING;

            return;

        }

    }

    void Jumping() {

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = inputHorizontal * horizontalSpeed;

        }

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        if (velocity.y == 0) {

            velocity.y = jumpSpeed;

        } else {

            velocity.y -= gravity * Time.deltaTime;

        }

        Vector2 platformPoint = GetNextPlatformPoint();
        Vector2 wallPoint = GetNextWallPoint();

        gameObject.transform.position = CalculatePlayerMovement(gameObject.transform.position, platformPoint, wallPoint);

        if (inputJumpsAvalible > 0 && inputJump) {

            inputJumpsAvalible -= 1;

            velocity.y = 0;

            state = STATE.PLAYER_JUMPING;

            return;

        }

        if (gameObject.transform.position.x == wallPoint.x) {

            state = STATE.PLAYER_WALL_SLIDE;

            return;

        }

        if (velocity.y <= 0) {

            state = STATE.PLAYER_FALLING;

            return;

        }

    }

    void WallSlide() {

        if (jumpDelayCoroutine != null) {

            StopCoroutine(jumpDelayCoroutine);

        }

        inputJumpsAvalible = maxAvalibleJumps;

        velocity.x = 0;

        if (velocity.y > 0) {

            velocity.y -= gravity * Time.deltaTime;

        } else {

            velocity.y = wallSlideSpeed;

        }

        Vector2 platformPoint = GetNextPlatformPoint();
        Vector2 wallPoint = GetNextWallPoint();

        gameObject.transform.position = CalculatePlayerMovement(gameObject.transform.position, platformPoint, wallPoint);

        if (inputJump) {

            state = STATE.PLAYER_WALL_JUMP;

            return;

        }

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            state = STATE.PLAYER_WALL_DISMOUNT;

            return;

        }

        if (gameObject.transform.position.y == platformPoint.y) {

            state = STATE.PLAYER_IDLE;

            return;

        }

    }

    void WallJump() {

        Flip();

        StartCoroutine(DisallowHorizontalMovement());

        velocity.x = horizontalDirection * horizontalSpeed;
        velocity.y = jumpSpeed;

        inputJumpsAvalible -= 1;

        state = STATE.PLAYER_JUMPING;

    }

    void WallDismount() {

        Flip();

        jumpDelayCoroutine = DisallowJump();

        StartCoroutine(jumpDelayCoroutine);

        velocity.x = inputHorizontal * horizontalSpeed;
        velocity.y = 0;

        state = STATE.PLAYER_FALLING;

    }

    void Flip() {

        Vector3 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

    }

    Vector2 CalculatePlayerMovement(Vector2 originalPosition, Vector2 platformPoint, Vector2 wallPoint) {

        Vector2 newPosition = new Vector2(originalPosition.x, originalPosition.y);

        newPosition = (Vector2) newPosition + velocity * Time.deltaTime;

        if (platformPoint.y < originalPosition.y) {

            newPosition.y = Mathf.Max(newPosition.y, platformPoint.y);

        }

        if (velocity.x > 0 && wallPoint.x > originalPosition.x) {

            newPosition.x = Mathf.Min(newPosition.x, wallPoint.x);

        } else if (velocity.x < 0 && wallPoint.x < originalPosition.x) {

            newPosition.x = Mathf.Max(newPosition.x, wallPoint.x);

        }

        return newPosition;

    }

    Vector2 GetNextPlatformPoint() {

        RaycastHit2D[] platforms = Physics2D.RaycastAll(platformTrigger.position, -gameObject.transform.up, Mathf.Infinity, platformLayerMask);

        foreach (RaycastHit2D platform in platforms) {

            Vector2 platformPoint = platform.point - (Vector2)(platformTrigger.localPosition * gameObject.transform.localScale.y);

            if (RoundFloat(platform.point.y) == RoundFloat(platform.collider.bounds.max.y)) {

                Debug.DrawLine(gameObject.transform.position, platformPoint, Color.green);

                return RoundVector2(platformPoint);

            }

        }

        return gameObject.transform.position;

    }

    Vector2 GetNextWallPoint() {

        RaycastHit2D[] walls = Physics2D.RaycastAll(wallTrigger.position, wallTrigger.right * horizontalDirection, Mathf.Infinity, wallLayerMask);

        foreach (RaycastHit2D wall in walls) {

            Vector2 wallPoint = wall.point - (Vector2)(wallTrigger.localPosition * gameObject.transform.localScale.x);

            if (RoundFloat(wall.point.x) == RoundFloat(wall.collider.bounds.min.x) ||
                RoundFloat(wall.point.x) == RoundFloat(wall.collider.bounds.max.x)) {

                Debug.DrawLine(gameObject.transform.position, wallPoint, Color.green);

                return RoundVector2(wallPoint);

            }

        }

        return gameObject.transform.position;

    }

    Vector2 RoundVector2(Vector2 vector, int digits = 2) {

        vector.x = RoundFloat(vector.x, digits);
        vector.y = RoundFloat(vector.y, digits);

        return vector;

    }

    float RoundFloat(float value, int digits = 2) {

        return (float) Math.Round((double) value, digits);

    }

    IEnumerator DisallowHorizontalMovement() {

        inputHorizontal = 0;

        inputHorizontalEnabled = false;

        yield return horizontalMovementDelay;

        inputHorizontalEnabled = true;

    }

    IEnumerator DisallowJump() {

        yield return jumpDelay;

        inputJumpsAvalible = 0;

    }

}
```
