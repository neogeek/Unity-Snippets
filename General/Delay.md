# Delay

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    private readonly float delayRate = 0.5f;

    private float nextTick;

    void Update() {

        if (Time.time > nextTick) {

            Debug.Log("tick");

            nextTick = Time.time + delayRate;

        }

    }

}
```
