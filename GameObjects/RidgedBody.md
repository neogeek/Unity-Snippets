# RidgedBody

## Apply Force to GameObject

```csharp
Rigidbody rb = gameObject.GetComponent<Rigidbody>();
rb.AddForce(new Vector3(0, 0, 10.0f) * 50.0f);
```

## Disable Collision on GameObject

Will disable the first attached collider component.

```csharp
Rigidbody rb = gameObject.GetComponent<Rigidbody>();
rb.GetComponent<Collider>().enabled = false;
```

Same as selecting the component from the game object directly.

```csharp
gameObject.GetComponent<Collider>().enabled = false;
```

## Setting Rigidbody to Kinematic

```csharp
gameObject.GetComponent<Rigidbody>().isKinematic = true;
```
