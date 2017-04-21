# Variables

- [Struct](/Variables/Struct.md)

## `Mathf.Floor`

```csharp
Debug.Log(Mathf.Floor(2 / 3)); // 0.0f
```

```csharp
Debug.Log(Mathf.FloorToInt(2 / 3)); // 0
```

## Random Values

```csharp
float randomPosition = Random.Range(1.0f, 25.0f);
```

## Select Random Item in an Array

```csharp
Debug.Log(randomColors[Random.Range(0, randomColors.Length)]);
```
