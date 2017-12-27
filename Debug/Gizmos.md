# Gizmos

## Draw Circle at Bottom Left Of GameObject

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void OnDrawGizmos() {

        Gizmos.color = Color.green;
        Gizmos.DrawWireSphere(gameObject.GetComponent<Renderer>().bounds.min, 1.0f);

    }

    void OnDrawGizmosSelected() {

        Gizmos.color = Color.red;
        Gizmos.DrawWireSphere(gameObject.GetComponent<Renderer>().bounds.max, 1.0f);

    }

}
```
