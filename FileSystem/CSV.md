# JSON

## Loading a CSV File

*Scripts/SampleController.cs*

```csharp
using System.Collections.Generic;
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start () {

        string[] vectors = System.IO.File.ReadAllText("Resources/Data/vectors.csv").Split(new char[] {'\n'}, System.StringSplitOptions.RemoveEmptyEntries);

        for (int i = 0; i < vectors.Length; i++) {

            string[] vector = vectors[i].Split(new char[] {','});

            Vector3 pos = new Vector3(float.Parse(vector[0]), float.Parse(vector[1]), float.Parse(vector[2]));

            Debug.Log(pos);

        }

    }

}
```

*Resources/Data/vectors.csv*

```json
1, 1, 1
2, 2, 2
3, 3, 3
```
