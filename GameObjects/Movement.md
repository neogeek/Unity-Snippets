# Movement

## Move and Rotate an Object in 3D

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    private readonly float moveSpeed = 10.0f;
    private readonly float rotateSpeed = 100.0f;

    void Update() {

        Vector3 rotation = new Vector3(0, Input.GetAxis("Horizontal"), 0);
        Vector3 moveVertical = gameObject.transform.forward * Input.GetAxis("Vertical");

        gameObject.transform.Rotate(rotation * rotateSpeed * Time.deltaTime);
        gameObject.transform.position += moveVertical * moveSpeed * Time.deltaTime;

    }

}
```

## Follow an Object in 3D

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    private Transform cameraTransform;
    private Vector3 cameraFollowOffset;

    void Awake() {

        cameraTransform = Camera.main.transform;

    }

    void Start() {

        cameraFollowOffset = cameraTransform.position - gameObject.transform.position;

    }

    void LateUpdate() {

        cameraTransform.position = gameObject.transform.position + cameraFollowOffset;

    }

}
```
