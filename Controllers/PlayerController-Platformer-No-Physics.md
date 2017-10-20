# PlayerController.cs (Platformer w/ no Physics)

```csharp
using System;
using System.Collections;
using UnityEngine;

[RequireComponent(typeof(BoxCollider2D))]
public class PlayerController : MonoBehaviour {

    public LayerMask leftLayerMask;
    public LayerMask rightLayerMask;
    public LayerMask topLayerMask;
    public LayerMask bottomLayerMask;

    public enum STATE {
        Idle,
        Running,
        Falling,
        Jumping,
        WallSlide,
        WallJump,
        WallDismount
    }

    [SerializeField]
    private STATE _state = STATE.Idle;

    public STATE state {
        get {
            return _state;
        }
        set {

            Debug.Log(string.Format("Switched from {0} to {1}.", _state, value));

            Invoke(value.ToString() + "Enter", 0);

            _state = value;

        }
    }

    private readonly float horizontalSpeed = 7.0f;
    private readonly float horizontalResistance = 0.02f;
    private readonly float lowJumpSpeed = 10.0f;
    private readonly float highJumpSpeed = 15.0f;
    private readonly float gravityMultiplier = 2f;
    private readonly float wallSlideSpeed = -2.0f;
    private readonly int maxAvalibleJumps = 2;

    private BoxCollider2D boxCollider;

    private Vector2 position = Vector2.zero;
    private Vector2 velocity = Vector2.zero;

    private float inputHorizontal = 0;
    private int inputJumpsAvalible = 0;

    private int horizontalDirection = 1;

    private Vector2? hitLeft;
    private Vector2? hitRight;
    private Vector2? hitTop;
    private Vector2? hitBottom;

    private float hitBottomBoxColliderFriction = 0;

    private bool _inputJumpPressed = false;
    private bool inputJumpPressed {
        get {
            return _inputJumpPressed;
        }
        set {

            if (value) {
                _inputJumpPressed = true;
            }

        }
    }

    private bool _inputJumpHeld = false;
    private bool inputJumpHeld {
        get {
            return _inputJumpHeld;
        }
        set {

            if (value) {
                _inputJumpHeld = true;
            }

        }
    }

    void Awake() {

        boxCollider = gameObject.GetComponent<BoxCollider2D>();

        position = gameObject.transform.position;

    }

    void Start() {

        state = STATE.Idle;

    }

    void Update() {

        if (Mathf.Abs(Input.GetAxisRaw("Horizontal")) > 0) {

            inputHorizontal = Input.GetAxisRaw("Horizontal");

        } else {

            inputHorizontal = 0;

        }

        inputJumpPressed = Input.GetButtonDown("Jump") || Input.GetKeyDown(KeyCode.Joystick1Button16);
        inputJumpHeld = Input.GetButton("Jump") || Input.GetKey(KeyCode.Joystick1Button16);

        gameObject.transform.position = position;

    }

    void FixedUpdate() {

        UpdateHitVectors();

        Invoke(state.ToString(), 0);

        Invoke("ResetInputVariables", 0);

    }

    private void IdleEnter() {

        inputJumpsAvalible = maxAvalibleJumps;

    }

    private void Idle() {

        if (velocity.x > 0) {

            velocity.x = Mathf.Max(velocity.x - hitBottomBoxColliderFriction, 0);

        } else if (velocity.x < 0) {

            velocity.x = Mathf.Min(velocity.x + hitBottomBoxColliderFriction, 0);

        }

        velocity.y = 0;

        position = Move();

        if (inputHorizontal == 1 && (!hitRight.HasValue || hitRight.HasValue && hitRight.Value.x > position.x) ||
            inputHorizontal == -1 && (!hitLeft.HasValue || hitLeft.HasValue && hitLeft.Value.x < position.x)) {

            state = STATE.Running;

            return;

        }

        if (!hitBottom.HasValue || (hitBottom.HasValue && hitBottom.Value.y < position.y)) {

            state = STATE.Falling;

            return;

        }

        if (inputJumpPressed) {

            state = STATE.Jumping;

            return;

        }

    }

    private void RunningEnter() {

        inputJumpsAvalible = maxAvalibleJumps;

    }

    private void Running() {

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = Mathf.Lerp(velocity.x, inputHorizontal * horizontalSpeed, Time.deltaTime * horizontalSpeed);

        }

        velocity.y = 0;

        position = Move();

        if (inputHorizontal == 0 || (hitRight.HasValue && hitRight.Value.x == position.x) ||
            (hitLeft.HasValue && hitLeft.Value.x == position.x)) {

            state = STATE.Idle;

            return;

        }

        if (!hitBottom.HasValue || (hitBottom.HasValue && hitBottom.Value.y < position.y)) {

            state = STATE.Falling;

            return;

        }

        if (inputJumpPressed) {

            state = STATE.Jumping;

            return;

        }

    }

    private void Falling() {

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = Mathf.Lerp(velocity.x, inputHorizontal * horizontalSpeed, Time.deltaTime * horizontalSpeed);

        } else if (velocity.x > 0) {

            velocity.x = Mathf.Max(velocity.x - horizontalResistance, 0);

        } else if (velocity.x < 0) {

            velocity.x = Mathf.Min(velocity.x + horizontalResistance, 0);

        }

        velocity.y += Physics2D.gravity.y * gravityMultiplier * Time.deltaTime;

        position = Move();

        if (inputJumpsAvalible > 0 && inputJumpPressed) {

            state = STATE.Jumping;

            return;

        }

        if ((hitRight.HasValue && hitRight.Value.x == position.x) ||
            (hitLeft.HasValue && hitLeft.Value.x == position.x)) {

            state = STATE.WallSlide;

            return;

        }

        if (hitBottom.HasValue && hitBottom.Value.y == position.y) {

            state = STATE.Idle;

            return;

        }

    }

    private void JumpingEnter() {

        inputJumpsAvalible -= 1;

        velocity.y = highJumpSpeed;

        Invoke("SetJumpSpeed", 0.1f);

    }

    private void SetJumpSpeed() {

        if (!inputJumpHeld) {

            velocity.y -= highJumpSpeed - lowJumpSpeed;

        }

    }

    private void Jumping() {

        if (Mathf.Abs(inputHorizontal) > 0 && inputHorizontal != horizontalDirection) {

            Flip();

        }

        if (Mathf.Abs(inputHorizontal) > 0) {

            velocity.x = Mathf.Lerp(velocity.x, inputHorizontal * horizontalSpeed, Time.deltaTime * horizontalSpeed);

        } else if (velocity.x > 0) {

            velocity.x = Mathf.Max(velocity.x - horizontalResistance, 0);

        } else if (velocity.x < 0) {

            velocity.x = Mathf.Min(velocity.x + horizontalResistance, 0);

        }

        velocity.y += Physics2D.gravity.y * Time.deltaTime;

        position = Move();

        if (inputJumpPressed && ((hitRight.HasValue && hitRight.Value.x == position.x) ||
                (hitLeft.HasValue && hitLeft.Value.x == position.x))) {

            state = STATE.WallJump;

            return;

        }

        if (inputJumpsAvalible > 0 && inputJumpPressed) {

            state = STATE.Jumping;

            return;

        }

        if ((hitTop.HasValue && hitTop.Value.y == position.y) || velocity.y <= 0) {

            velocity.y = 0;

            state = STATE.Falling;

            return;

        }

    }

    private void WallSlideEnter() {

        inputJumpsAvalible = maxAvalibleJumps;

        velocity.y = 0;

    }

    private void WallSlide() {

        velocity.x = 0;

        if (inputHorizontal == 0) {

            velocity.y += Physics2D.gravity.y * gravityMultiplier * Time.deltaTime;

        } else {

            velocity.y = wallSlideSpeed;

        }

        position = Move();

        if (inputJumpPressed) {

            state = STATE.WallJump;

            return;

        }

        if ((!hitRight.HasValue || hitRight.Value.x != position.x) &&
            (!hitLeft.HasValue || hitLeft.Value.x != position.x)) {

            state = STATE.Falling;

            return;

        }

        if (inputHorizontal == -1 && !hitLeft.HasValue || inputHorizontal == 1 && !hitRight.HasValue) {

            state = STATE.WallDismount;

            return;

        }

        if (hitBottom.HasValue && hitBottom.Value.y == position.y) {

            state = STATE.Idle;

            return;

        }

    }

    private void WallJump() {

        Flip();

        velocity.x = horizontalDirection * horizontalSpeed;

        state = STATE.Jumping;

    }

    private void WallDismount() {

        velocity.x = inputHorizontal * horizontalSpeed;

        state = STATE.Falling;

    }

    private void Flip() {

        Vector3 scale = gameObject.transform.localScale;
        horizontalDirection *= -1;
        scale.x *= -1;
        gameObject.transform.localScale = scale;

        velocity.x = 0;

    }

    private void UpdateHitVectors() {

        Bounds colliderBounds = boxCollider.bounds;

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

        if (hitLeftRay && hitLeftRay.collider.bounds.min.x <= colliderBounds.max.x) {

            hitLeft = new Vector2(hitLeftRay.collider.bounds.max.x + colliderBounds.extents.x, 0);

        } else {

            hitLeft = null;

        }

        if (hitRightRay && hitRightRay.collider.bounds.max.x >= colliderBounds.min.x) {

            hitRight = new Vector2(hitRightRay.collider.bounds.min.x - colliderBounds.extents.x, 0);

        } else {

            hitRight = null;

        }

        if (hitTopRay && hitTopRay.collider.bounds.min.y >= colliderBounds.max.y) {

            hitTop = new Vector2(0, hitTopRay.collider.bounds.min.y - colliderBounds.extents.y);

        } else {

            hitTop = null;

        }

        if (hitBottomRay && hitBottomRay.collider.bounds.max.y <= colliderBounds.min.y) {

            hitBottom = new Vector2(0, hitBottomRay.collider.bounds.max.y + colliderBounds.extents.y);

            hitBottomBoxColliderFriction = hitBottomRay.collider.friction;

        } else {

            hitBottom = null;

            hitBottomBoxColliderFriction = 0;

        }

    }

    private Vector2 Move() {

        Vector2 nextPosition = position;

        nextPosition += velocity * Time.deltaTime;

        if (hitLeft.HasValue) {

            nextPosition.x = Mathf.Max(nextPosition.x, hitLeft.Value.x);

        }

        if (hitRight.HasValue) {

            nextPosition.x = Mathf.Min(nextPosition.x, hitRight.Value.x);

        }

        if (hitTop.HasValue) {

            nextPosition.y = Mathf.Min(nextPosition.y, hitTop.Value.y);

        }

        if (hitBottom.HasValue) {

            nextPosition.y = Mathf.Max(nextPosition.y, hitBottom.Value.y);

        }

        return nextPosition;

    }

    private void ResetInputVariables() {

        _inputJumpPressed = false;
        _inputJumpHeld = false;

    }

}
```
