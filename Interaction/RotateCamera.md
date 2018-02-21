# Rotate Camera

## Rotate Camera With Mouse

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour
{

    private readonly float rotateSpeed = 5f;

    private void LateUpdate()
    {

        if (Input.GetMouseButton(0))
        {

            gameObject.transform.rotation = Quaternion.Euler(
                Input.GetAxis("Mouse Y") * rotateSpeed + gameObject.transform.rotation.eulerAngles.x,
                -Input.GetAxis("Mouse X") * rotateSpeed + gameObject.transform.rotation.eulerAngles.y,
                0
            );

        }

    }

}
```
