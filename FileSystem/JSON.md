# JSON

## Loading a JSON File

*Scripts/LoadDataController.cs*

```csharp
using UnityEngine;

[System.Serializable]
public struct Vector {
    public string timestamp;
    public int x;
    public int y;
    public int z;
}

[System.Serializable]
public struct VectorList {
    public Vector[] vectors;
}

public class LoadDataController : MonoBehaviour {

    void Start () {

        VectorList list = JsonUtility.FromJson<VectorList>((Resources.Load("Data/vectors") as TextAsset).text);

        Debug.Log(JsonUtility.ToJson(list));

    }

}
```

*Resources/Data/vectors.json*

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
