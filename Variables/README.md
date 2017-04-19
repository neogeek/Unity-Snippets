# Variables

## Random Values

```csharp
Vector3 randomPosition = new Vector3(
    Mathf.Floor(Random.Range(1.0f, 25.0f)),
    Mathf.Floor(Random.Range(1.0f, 25.0f)),
    Mathf.Floor(Random.Range(1.0f, 25.0f))
);
```

## Select Random Item in an Array

```csharp
Debug.Log(randomColors[Random.Range(0, randomColors.Length)]);
```
