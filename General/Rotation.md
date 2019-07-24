# Rotation

## Rotate 2D Canvas Object to Degree

```csharp

public static Quaternion RotateDegrees(float degree)
{
    gameObject.transform.rotation = Quaternion.AngleAxis(degree, Vector3.forward);
}

```

## Rotate 2D Canvas Object Towards 3D Position

```csharp
public static Quaternion RotateTowardsTarget(Vector3 origin, Vector3 target)
{
    Vector2 angle = target - origin;

    return Quaternion.AngleAxis(Mathf.Atan2(angle.y, angle.x) * Mathf.Rad2Deg, Vector3.forward);
}
```

## Place Object at Position in Circle Based on Angle

```csharp
public static Vector3 PlaceObjectOnCircle(Vector3 position, float degree) {
    var angle = 90;
    var radius = 100;
    return new Vector3(Mathf.Cos(angle) * radius, Mathf.Sin(angle) * radius, angle) + position;
}
```
