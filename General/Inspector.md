# Inspector

## Range Slider

```csharp
using UnityEngine;

public class InspectorExample : MonoBehaviour {

    [Range(0.1f, 1.0f)]
    public float offset = 0.25f;

}
```

## Headers and Spaces

```csharp
using UnityEngine;

public class InspectorExample : MonoBehaviour {

    [Header("General Information")]
    public string name = "Example";

    [Space]

    [Header("Position")]
    public Vector3 position = new Vector3(0.0f, 0.0f, 0.0f);

}
```
