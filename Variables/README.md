# Variables

- [Date](/Variables/Date.md)
- [Struct](/Variables/Struct.md)

## `int.Parse`

```csharp
Debug.Log(int.Parse("1")); // 1
```

```csharp
Debug.Log(int.Parse("1")); // 1
```

## `float.Parse`

```csharp
Debug.Log(float.Parse("1.5")); // 1.5f
```

## `Mathf.Ceiling`

```csharp
Debug.Log(Mathf.Ceil(10f)); // 10.0f
```

```csharp
Debug.Log(Mathf.Ceil(10.5f)); // 10.0f
```

```csharp
Debug.Log(Mathf.Ceil(10.6f)); // 11.0f
```

## `Mathf.Floor`

```csharp
Debug.Log(Mathf.Floor(10f)); // 10.0f
```

```csharp
Debug.Log(Mathf.Floor(10.5f)); // 10.0f
```

## `Mathf.FloorToInt`

```csharp
Debug.Log(Mathf.FloorToInt(10.6f)); // 10
```

## Random Values

```csharp
float randomPosition = Random.Range(1.0f, 25.0f);
```

## Select Random Item in an Array

```csharp
Debug.Log(randomColors[Random.Range(0, randomColors.Length)]);
```
