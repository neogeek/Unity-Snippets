# Line Renderer

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start () {

        LineRenderer lineRenderer = gameObject.AddComponent<LineRenderer>();

        lineRenderer.widthMultiplier = 0.1f;
        lineRenderer.positionCount = 5;

        lineRenderer.SetPosition(0, new Vector3(0, 0, 0));
        lineRenderer.SetPosition(1, new Vector3(1, 1, 1));
        lineRenderer.SetPosition(2, new Vector3(2, 1, 1));
        lineRenderer.SetPosition(3, new Vector3(1, 0, 0));
        lineRenderer.SetPosition(4, new Vector3(0, 0, 0));

    }

}
```
