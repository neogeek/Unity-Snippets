# RidgedBody

## Apply Force to GameObject

```csharp
Rigidbody rb = newObject.GetComponent<Rigidbody>();
rb.AddRelativeForce(new Vector3(0, 0, 10.0f) * 50.0f);
```

## Disable Collision on GameObject

```csharp
Rigidbody rb = newObject.GetComponent<Rigidbody>();
rb.GetComponent<Collider>().enabled = false;
```

## Disable Rigidbody

```csharp
gameObject.GetComponent<Rigidbody>().isKinematic = true;
```
