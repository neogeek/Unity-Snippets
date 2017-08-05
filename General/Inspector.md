# Inspector

## Range Slider

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    [Range(0.1f, 1.0f)]
    public float offset = 0.25f;

}
```

## Headers and Spaces

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    [Header("General Information")]
    public string label = "Example";

    [Space]

    [Header("Position")]
    public Vector3 position = new Vector3(0.0f, 0.0f, 0.0f);

}
```

## Tooltip

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    [Tooltip("Offset of GameObject from parent")]
    public float offset = 0.25f;

}
```

## Textarea

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    [TextArea]
    public string description;

}
```
