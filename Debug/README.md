# Debug

- [Gizmos](/Debug/Gizmos.md)
- [Log](/Debug/Log.md)

## Debug.DrawRay

```csharp
public class SampleController : MonoBehaviour {

    void Update() {

        Debug.DrawRay(gameObject.transform.position, gameObject.transform.forward * 10.0f, Color.green);

    }

}
```
