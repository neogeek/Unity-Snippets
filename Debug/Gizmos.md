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

## Attach to GameObject for Gizmos Created Through the Inspector

```csharp
using UnityEngine;

public class Gizmo : MonoBehaviour {

    public enum TYPE {
        GIZMO_NONE,
        GIZMO_SPHERE
    }

    public TYPE type = TYPE.GIZMO_NONE;

    public Color color = Color.green;
    public Vector3 vector = Vector3.zero;
    public float size = 0.2f;

    void OnDrawGizmos() {

        Gizmos.color = color;

        if (type == TYPE.GIZMO_SPHERE) {

            Gizmos.DrawWireSphere(gameObject.transform.position + vector, size);

        }

    }

}
```
