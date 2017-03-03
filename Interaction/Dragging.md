# Dragging GameObjects

## Drag From Center

```csharp
using UnityEngine;
using System.Collections;

public class GameObjectController : MonoBehaviour {

    private Vector2 GetMousePosition () {

        return Camera.main.ScreenToWorldPoint(new Vector3(
            Input.mousePosition.x,
            Input.mousePosition.y,
            gameObject.transform.position.z
        ));

    }

    void OnMouseDrag () {

        gameObject.transform.position = GetMousePosition();

    }

}
```
