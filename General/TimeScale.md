# TimeScale

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void OnTriggerEnter(Collider other) {

        if (other.gameObject.name == "SlowDownTime") {

            Time.timeScale = 0.25f;
            Time.fixedDeltaTime = Time.timeScale * 0.02f;

        }

    }

    void OnTriggerExit(Collider other) {

        if (other.gameObject.name == "SlowDownTime") {

            Time.timeScale = 1.0f;
            Time.fixedDeltaTime = Time.timeScale * 0.02f;

        }

    }

}
```
