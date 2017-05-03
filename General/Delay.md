# Delay

```csharp
using UnityEngine;

public class DelayExample : MonoBehaviour {

    private float delayRate = 0.25f;

    private float nextTick;

    void Update () {

        if (Time.time > nextTick) {

            Debug.Log("tick");

            nextTick = Time.time + delayRate;

        }

    }

}
```
