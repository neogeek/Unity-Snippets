# Click

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Update() {

        if (Input.GetMouseButtonDown(0) &&
            Physics.Raycast(Camera.main.ScreenPointToRay (Input.mousePosition), Mathf.Infinity)) {

            Debug.Log("click");

        }

    }

}
```
