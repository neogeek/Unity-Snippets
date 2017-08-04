# Line Renderer

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start() {

        LineRenderer lineRenderer = gameObject.AddComponent<LineRenderer>();

        lineRenderer.material.color = Color.red;
        lineRenderer.widthMultiplier = 0.1f;
        lineRenderer.positionCount = 5;

        Vector3[] positions = new Vector3[5];

        positions[0] = new Vector3(0, 0, 0);
        positions[1] = new Vector3(1, 1, 1);
        positions[2] = new Vector3(2, 1, 1);
        positions[3] = new Vector3(1, 0, 0);
        positions[4] = new Vector3(0, 0, 0);

        lineRenderer.SetPositions(positions);

    }

}
```
