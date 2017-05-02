# Movement

## Move and Rotate an Object in 3D

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    private float moveSpeed = 10.0f;
    private float rotateSpeed = 100.0f;
    private float threshold = 0.5f;

    void Update () {

        Vector3 rotation = new Vector3(0, Input.GetAxis("Horizontal"),  0);

        if (Input.GetAxis("Vertical") > threshold) {

            transform.position += transform.forward * moveSpeed * Time.deltaTime;

        } else if (Input.GetAxis("Vertical") < -threshold) {

            transform.position -= transform.forward * moveSpeed * Time.deltaTime;

        }

        transform.Rotate(rotation * rotateSpeed * Time.deltaTime);

    }

}
```

## Follow an Object in 3D

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

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
