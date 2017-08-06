# Invoke

## Run Function Once

```csharp
Invoke("FireProjectile", 2.0f);
```

## Run Function Multiple Times

```csharp
InvokeRepeating("FireProjectile", 2.0f, 1.0f);
```

## Cancel

```csharp
CancelInvoke("FireProjectile");
```

## Check for Active Invocation

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Update() {

        if (Input.GetKey("space") && !IsInvoking("FireProjectile")) {

            Invoke("FireProjectile", 1.0f);

        }

    }

    void FireProjectile() {

        Debug.Log("Fire projectile");

    }

}
```
