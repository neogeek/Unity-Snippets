# CameraFollow

```csharp
using UnityEngine;

public class CameraFollow : MonoBehaviour {

    private Transform target;

    private Camera mainCamera;

    private float dampRate = 0.3f;
    private Vector3 velocity = Vector3.zero;

    void Awake () {

        mainCamera = Camera.main;

    }

    void Update () {

        if (target) {

            mainCamera.transform.position = Vector3.SmoothDamp(
                mainCamera.transform.position, new Vector3(
                    target.transform.position.x,
                    target.transform.position.y,
                    mainCamera.transform.position.z
                ),
                ref velocity,
                dampRate
            );

        }

    }

}
```