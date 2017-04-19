# Game Objects

- [Renderer](/GameObjects/Renderer.md)
- [RidgedBody](/GameObjects/RidgedBody.md)

## Dynamically Generate Objects

```csharp
GameObject newObject = Instantiate(objectToGenerate, new Vector3(0, 0, 0), Quaternion.identity);
```

## Find Child GameObject

```csharp
GameObject cubeObj = gameObject.transform.FindChild("Cube").gameObject;
```

## Add a `public` Color Array to a GameObject

```csharp
using UnityEngine;

public class ExampleScript : MonoBehaviour {
	public Color[] randomColors;
}
```

## Access `public` Variable on Another GameObject

```csharp
anotherGameObject.GetComponent<ScriptName>().currentActiveState = false;
```

