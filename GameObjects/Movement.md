# Movement

## Player Move Controller

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour {

    private float moveSpeed = 10.0f;
    private float rotateSpeed = 100.0f;

    void Update () {

        Vector3 rotation = new Vector3(0, Input.GetAxis("Horizontal"),  0);

        if (Input.GetAxis("Vertical") > 0.5) {

            transform.position += transform.forward * moveSpeed * Time.deltaTime;

        } else if (Input.GetAxis("Vertical") < -0.5) {

            transform.position -= transform.forward * moveSpeed * Time.deltaTime;

        }

        transform.Rotate(rotation * rotateSpeed * Time.deltaTime);

    }

}
```

## Camera Follow Controller

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour {

    public Transform cameraTransform;

    private Vector3 cameraFollowOffset;

    void Start () {

        cameraFollowOffset = cameraTransform.position - transform.position;

    }

    void LateUpdate () {

        cameraTransform.position = transform.position + cameraFollowOffset;

    }

}
```
