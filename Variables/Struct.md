# `struct`

## Using a `struct` with a List

```csharp
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public struct Platform {
    public float posX;
    public float posY;
    public float posZ;
    public Vector3 position {
        get {
            return new Vector3(posX, posY, posZ);
        }
        set {
            posX = value.x;
            posY = value.y;
            posZ = value.z;
        }
    }
    public float scaleX;
    public float scaleY;
    public float scaleZ;
    public Vector3 scale {
        get {
            return new Vector3(scaleX, scaleY, scaleZ);
        }
        set {
            scaleX = value.x;
            scaleY = value.y;
            scaleZ = value.z;
        }
    }
    public float rotationX;
    public float rotationY;
    public float rotationZ;
    public Quaternion rotation {
        get {
            return Quaternion.Euler(rotationX, rotationY, rotationZ);
        }
        set {
            rotationX = value.x;
            rotationY = value.y;
            rotationZ = value.z;
        }
    }
}

public class SampleController : MonoBehaviour {

    private List<Platform> platforms = new List<Platform>();

    void Start() {

        Platform examplePlatform = new Platform();

        examplePlatform.position = Vector3.zero;

        platforms.Add(examplePlatform);

        Debug.Log(platforms[0].position);

    }

}
```
