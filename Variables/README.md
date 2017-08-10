# Variables

- [Date](/Variables/Date.md)
- [Struct](/Variables/Struct.md)

## Cast Value To Integer

```csharp
Debug.Log (int.Parse ("1")); // 1
```

```csharp
Debug.Log (int.Parse ("1.5")); // 1
```

## Cast Value To Float

```csharp
Debug.Log (float.Parse ("1.5")); // 1.5f
```

## Mathf.Ceiling

```csharp
Debug.Log (Mathf.Ceil (10f)); // 10.0f
```

```csharp
Debug.Log (Mathf.Ceil (10.5f)); // 10.0f
```

```csharp
Debug.Log (Mathf.Ceil (10.6f)); // 11.0f
```

## Mathf.Floor

```csharp
Debug.Log (Mathf.Floor (10f)); // 10.0f
```

```csharp
Debug.Log (Mathf.Floor (10.5f)); // 10.0f
```

## Mathf.FloorToInt

```csharp
Debug.Log (Mathf.FloorToInt (10f)); // 10
```

```csharp
Debug.Log (Mathf.FloorToInt (10.6f)); // 10
```

## Clamp Number

```csharp
// Mathf.Clamp(float value, float min, float max);
Debug.Log (Mathf.Clamp (10.0f, 1.0f, 5.0f)); // 5.0f
```

## Random Values

```csharp
Debug.Log (Random.Range (1.0f, 25.0f));
```

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    private System.Random rnd;

    void Awake() {

        rnd = new System.Random();

    }

    void Update() {

        Debug.Log(rnd.Next(0, 10));
        Debug.Log(rnd.Next(0, 10));
        Debug.Log(rnd.Next(0, 10));

    }

}
```

## Select Random Item in an Array

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    public Color[] randomColors;

    void Update() {

        Debug.Log(randomColors[Random.Range(0, randomColors.Length)]);

    }

}
```
