# CalculateParentBounds

```csharp
using UnityEngine;

public struct LocalBounds {
    public Vector3 min;
    public Vector3 max;
}

public class SampleController : MonoBehaviour {

    LocalBounds CalculateParentBounds(GameObject parentObj) {

        LocalBounds caluclatedBounds = new LocalBounds();

        for (int i = 0; i < parentObj.transform.childCount; i++) {

            GameObject child = parentObj.transform.GetChild(i).gameObject;

            Bounds bounds = child.GetComponent<Renderer>().bounds;
            Vector3 position = child.transform.position;

            caluclatedBounds.min = Vector3.Min(caluclatedBounds.min, bounds.min);
            caluclatedBounds.max = Vector3.Max(caluclatedBounds.max, bounds.max);

        }

        return caluclatedBounds;

    }

    void OnDrawGizmosSelected() {

        LocalBounds bounds = CalculateParentBounds(gameObject);

        Gizmos.color = Color.red;

        Gizmos.DrawWireSphere(bounds.min, 0.25f);
        Gizmos.DrawWireSphere(bounds.max, 0.25f);

    }

}
```
