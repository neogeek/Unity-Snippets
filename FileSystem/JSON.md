# JSON

## Loading a JSON File

*Scripts/SampleController.cs*

```csharp
using UnityEngine;

[System.Serializable]
public struct Vector {
    public float x;
    public float y;
    public float z;
    public Vector3 position {
        get {
            return new Vector3(x, y, z);
        }
        set {
            x = value.x;
            y = value.y;
            z = value.z;
        }
    }
}

[System.Serializable]
public struct VectorList {
    public Vector[] vectors;
}

public class SampleController : MonoBehaviour {

    void Start() {

        VectorList list = JsonUtility.FromJson<VectorList>((Resources.Load("vectors") as TextAsset).text);

        Debug.Log(list.vectors[0].position);

        Debug.Log(JsonUtility.ToJson(list));

    }

}
```

*Resources/vectors.json*

```json
{
    "vectors": [
        {
            "x": 0,
            "y": 0,
            "z": 0
        },
        {
            "x": 1,
            "y": 1,
            "z": 1
        },
        {
            "x": 2,
            "y": 2,
            "z": 2
        }
    ]
}
```
