# Dragging

## Drag GameObject From Center

```csharp
using UnityEngine;

public class DraggingObjectController : MonoBehaviour {

    private Vector3 GetMousePosition () {

        return Camera.main.ScreenToWorldPoint(new Vector3(
            Input.mousePosition.x,
            Input.mousePosition.y,
            Camera.main.WorldToScreenPoint(gameObject.transform.position).z
        ));

    }

    void OnMouseDrag () {

        gameObject.transform.position = GetMousePosition();

    }

}
```
