# PlayerController.cs (Platformer w/ no Physics)

```csharp
using System;
using System.Collections;
using UnityEngine;

public class PlayerController : MonoBehaviour {

    public LayerMask leftLayerMask;
    public LayerMask rightLayerMask;
    public LayerMask topLayerMask;
    public LayerMask bottomLayerMask;

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
    private readonly float jumpSpeed = 14.0f;
    private readonly int maxAvalibleJumps = 2;

    private Vector2 velocity = Vector2.zero;
    private int horizontalDirection = 1;

    private float inputHorizontal = 0;
    private int inputJumpsAvalible = 0;

    private Vector2? hitLeft;
    private Vector2? hitRight;
    private Vector2? hitTop;
    private Vector2? hitBottom;

    private bool _inputJump = false;
    private bool inputJump {
        get {
            return _inputJump;
        }
        set {

            if (value) {
                _inputJump = true;
            }

        }
    }

    void Update() {

        if (Mathf.Abs(Input.GetAxisRaw("Horizontal")) > 0) {

            inputHorizontal = Mathf.Sign(Input.GetAxisRaw("Horizontal"));

        } else {

            inputHorizontal = 0;

        }

        inputJump = Input.GetButtonDown("Jump") || Input.GetKeyDown(KeyCode.Joystick1Button16);

    }

    void FixedUpdate() {

        UpdateHitVectors();

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

        ResetInputVariables();

    }

    void Idle() {

        inputJumpsAvalible = maxAvalibleJumps;

        velocity = Vector2.zero;

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        if ((inputHorizontal == 1 && (!hitRight.HasValue || hitRight.HasValue && hitRight.Value.x > gameObject.transform.position.x)) ||
            (inputHorizontal == -1 && (!hitLeft.HasValue || hitLeft.HasValue && hitLeft.Value.x < gameObject.transform.position.x))) {

            state = STATE.PLAYER_RUNNING;

            return;

        }

        if (hitBottom.HasValue && hitBottom.Value.y < gameObject.transform.position.y) {

            state = STATE.PLAYER_FALLING;

            return;

        }

        if (inputJump) {

            JumpingEnter();

            state = STATE.PLAYER_JUMPING;

            return;

        }

    }

    void Running() {

        inputJumpsAvalible = maxAvalibleJumps;

        velocity.x = inputHorizontal * horizontalSpeed;
        velocity.y = 0;

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        gameObject.transform.position = Move();

        if (hitRight.HasValue && hitRight.Value.x == gameObject.transform.position.x ||
            hitLeft.HasValue && hitLeft.Value.x == gameObject.transform.position.x) {

            state = STATE.PLAYER_IDLE;

            return;

        }

        if (!hitBottom.HasValue || hitBottom.HasValue && hitBottom.Value.y < gameObject.transform.position.y) {

            state = STATE.PLAYER_FALLING;

            return;

        }

        if (inputJump) {

            JumpingEnter();

            state = STATE.PLAYER_JUMPING;

            return;

        }

        if (inputHorizontal == 0) {

            state = STATE.PLAYER_IDLE;

            return;

        }

    }

    void FallingEnter() {

        velocity.y = 0;

    }

    void Falling() {

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = inputHorizontal * horizontalSpeed;

        }

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        velocity.y -= gravity * Time.deltaTime;

        gameObject.transform.position = Move();

        if (inputJumpsAvalible > 0 && inputJump) {

            JumpingEnter();

            state = STATE.PLAYER_JUMPING;

            return;

        }

        if (hitRight.HasValue && hitRight.Value.x == gameObject.transform.position.x ||
            hitLeft.HasValue && hitLeft.Value.x == gameObject.transform.position.x) {

            WallSlideEnter();

            state = STATE.PLAYER_WALL_SLIDE;

            return;

        }

        if (hitBottom.HasValue && hitBottom.Value.y == gameObject.transform.position.y) {

            state = STATE.PLAYER_RUNNING;

            return;

        }

    }

    void JumpingEnter() {

        inputJumpsAvalible -= 1;

        velocity.y = jumpSpeed;

    }

    void Jumping() {

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = inputHorizontal * horizontalSpeed;

        }

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        velocity.y -= gravity * Time.deltaTime;

        gameObject.transform.position = Move();

        if (inputJumpsAvalible > 0 && inputJump) {

            JumpingEnter();

            state = STATE.PLAYER_JUMPING;

            return;

        }

        if (hitRight.HasValue && hitRight.Value.x == gameObject.transform.position.x ||
            hitLeft.HasValue && hitLeft.Value.x == gameObject.transform.position.x) {

            WallSlideEnter();

            state = STATE.PLAYER_WALL_SLIDE;

            return;

        }

        if (hitTop.HasValue && hitTop.Value.y == gameObject.transform.position.y) {

            FallingEnter();

            state = STATE.PLAYER_FALLING;

            return;

        }

        if (velocity.y <= 0) {

            state = STATE.PLAYER_FALLING;

            return;

        }

    }

    void WallSlideEnter() {

        inputJumpsAvalible = maxAvalibleJumps;

        velocity.x = 0;

    }

    void WallSlide() {

        if (velocity.y > 0) {

            velocity.y -= gravity * Time.deltaTime;

        } else {

            velocity.y = wallSlideSpeed;

        }

        gameObject.transform.position = Move();

        if (inputJump) {

            state = STATE.PLAYER_WALL_JUMP;

            return;

        }

        if ((!hitRight.HasValue || hitRight.Value.x != gameObject.transform.position.x) &&
            (!hitLeft.HasValue || hitLeft.Value.x != gameObject.transform.position.x)) {

            state = STATE.PLAYER_FALLING;

            return;

        }

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            state = STATE.PLAYER_WALL_DISMOUNT;

            return;

        }

        if (hitBottom.HasValue && hitBottom.Value.y == gameObject.transform.position.y) {

            state = STATE.PLAYER_IDLE;

            return;

        }

    }

    void WallJump() {

        Flip();

        velocity.x = horizontalDirection * horizontalSpeed;

        JumpingEnter();

        state = STATE.PLAYER_JUMPING;

    }

    void WallDismount() {

        Flip();

        velocity.x = inputHorizontal * horizontalSpeed;

        FallingEnter();

        state = STATE.PLAYER_FALLING;

    }

    void Flip() {

        Vector3 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

    }

    void UpdateHitVectors() {

        Bounds colliderBounds = gameObject.GetComponent<BoxCollider2D>().bounds;

        Vector2 rayCastSize = colliderBounds.size * 0.95f;

        RaycastHit2D hitLeftRay = Physics2D.BoxCast(
            new Vector2(colliderBounds.min.x - colliderBounds.extents.x, colliderBounds.center.y),
            rayCastSize,
            0f,
            Vector2.left,
            0f,
            leftLayerMask
        );

        RaycastHit2D hitRightRay = Physics2D.BoxCast(
            new Vector2(colliderBounds.max.x + colliderBounds.extents.x, colliderBounds.center.y),
            rayCastSize,
            0f,
            Vector2.right,
            0f,
            rightLayerMask
        );

        RaycastHit2D hitTopRay = Physics2D.BoxCast(
            new Vector2(colliderBounds.center.x, colliderBounds.max.y + colliderBounds.extents.y),
            rayCastSize,
            0f,
            Vector2.up,
            0f,
            topLayerMask
        );

        RaycastHit2D hitBottomRay = Physics2D.BoxCast(
            new Vector2(colliderBounds.center.x, colliderBounds.min.y - colliderBounds.extents.y),
            rayCastSize,
            0f,
            Vector2.down,
            0f,
            bottomLayerMask
        );

        Vector2 position = gameObject.transform.position;

        if (hitLeftRay) {

            hitLeft = new Vector2(hitLeftRay.collider.bounds.max.x + colliderBounds.extents.x, 0);

        } else {

            hitLeft = null;

        }

        if (hitRightRay) {

            hitRight = new Vector2(hitRightRay.collider.bounds.min.x - colliderBounds.extents.x, 0);

        } else {

            hitRight = null;

        }

        if (hitTopRay) {

            hitTop = new Vector2(0, hitTopRay.collider.bounds.min.y - colliderBounds.extents.y);

        } else {

            hitTop = null;

        }

        if (hitBottomRay) {

            hitBottom = new Vector2(0, hitBottomRay.collider.bounds.max.y + colliderBounds.extents.y);

        } else {

            hitBottom = null;

        }

    }

    Vector2 Move() {

        Vector2 position = gameObject.transform.position;

        position += velocity * Time.deltaTime;

        if (hitLeft.HasValue) {

            position.x = Mathf.Max(position.x, hitLeft.Value.x);

        }

        if (hitRight.HasValue) {

            position.x = Mathf.Min(position.x, hitRight.Value.x);

        }

        if (hitTop.HasValue) {

            position.y = Mathf.Min(position.y, hitTop.Value.y);

        }

        if (hitBottom.HasValue) {

            position.y = Mathf.Max(position.y, hitBottom.Value.y);

        }

        return position;

    }

    void ResetInputVariables() {

        _inputJump = false;

    }

}
```
