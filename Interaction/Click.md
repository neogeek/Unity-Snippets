# Click

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Update() {

        RaycastHit hitInfo;

        bool hit = Physics.Raycast(Camera.main.ScreenPointToRay(Input.mousePosition), out hitInfo, Mathf.Infinity);

        if (Input.GetMouseButtonDown(0) && hit && hitInfo.collider.gameObject == gameObject) {

            Debug.Log(gameObject.name);

        }

    }

}
```
