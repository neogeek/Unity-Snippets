# ScreenShake

```csharp
using UnityEngine;

public class ScreenShake : MonoBehaviour {

    private Vector3 originalPosition;

    private float currentIntensity = 0;
    private float currentDuraton = 0;

    void Awake() {

        originalPosition = gameObject.transform.position;

    }

    void Update() {

        if (currentDuraton > 0) {

            gameObject.transform.position = originalPosition + Random.insideUnitSphere * currentIntensity;

            currentDuraton = Mathf.Max(currentDuraton - Time.deltaTime, 0);

        } else {

            gameObject.transform.position = originalPosition;

        }

    }

    public void Shake(float duration = 1.0f, float intensity = 0.2f) {

        currentIntensity = intensity;
        currentDuraton = duration;

    }

}
```

```csharp
public class SampleController : MonoBehaviour {

    public ScreenShake screenShake;

    void Update() {

        if (Input.GetMouseButtonDown(0) &&
            Physics.Raycast(Camera.main.ScreenPointToRay (Input.mousePosition), Mathf.Infinity)) {

            screenShake.Shake();

        }

    }

}
```
