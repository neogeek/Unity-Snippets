# Camera

```csharp
public static bool IsVisibleByCamera(GameObject gameObject, Camera camera)
{
    var position = camera.WorldToViewportPoint(gameObject.transform.position);

    return position.x > 0 && position.x < 1 && position.y > 0 && position.y < 1 && position.z > 0;
}
```
