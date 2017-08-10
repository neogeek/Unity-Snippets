# Game Objects

- [Movement](/GameObjects/Movement.md)
- [Renderer](/GameObjects/Renderer.md)
- [RidgedBody](/GameObjects/RidgedBody.md)

## Dynamically Generate Objects

```csharp
GameObject newObject = Instantiate(objectToGenerate, Vector3.zero, Quaternion.identity);
```

## Find Child GameObject

```csharp
GameObject cubeObj = gameObject.transform.Find("Cube").gameObject;
```

## Add a public Color Array to a GameObject

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    public Color[] randomColors;

}
```

## Access public Variable on Another GameObject

```csharp
anotherGameObject.GetComponent<ScriptName>().currentActiveState = false;
```

## Rotate GameObject with Vector3

```csharp
newObject.transform.rotation = Quaternion.Euler(0, 0, -45.0f);
```

## Instantiate Object as a Child and Position it Relative to it's Parent Object

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    public GameObject gameObj;

    void Start() {

        GameObject tempObj = Instantiate(gameObj, gameObject.transform.position, Quaternion.identity);

        tempObj.transform.SetParent(gameObject.transform);

    }

}
```
