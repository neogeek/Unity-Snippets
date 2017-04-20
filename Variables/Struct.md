# `struct`

## Using a `struct` with a List

```csharp
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public struct PositionalData {
    public string timestamp;
    public float QuatW;
    public float QuatX;
    public float QuatY;
    public float QuatZ;
    public float AccelX;
    public float AccelY;
    public float AccelZ;
    public float GravX;
    public float GravY;
    public float GravZ;

}

public class DebugController : MonoBehaviour {

    private List<PositionalData> generatedPositionalData = new List<PositionalData>();

    void Update () {

        PositionalData temp = new PositionalData();

        temp.QuatW = transform.rotation.w;
        temp.QuatX = transform.rotation.x;
        temp.QuatY = transform.rotation.y;
        temp.QuatZ = transform.rotation.z;

        generatedPositionalData.Add(temp);

    }

}
```
