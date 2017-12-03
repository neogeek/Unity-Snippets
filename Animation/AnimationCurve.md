# AnimationCurve

![](https://media.giphy.com/media/3ohs7VvorTmN1W7bOM/giphy.gif)

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    public AnimationCurve animationCurve = AnimationCurve.Linear(0, 0, 1, 1);
    public bool shouldAnimate = true;

    private Vector3 startSize;
    private float activeTime = 0;

    void Awake() {

        startSize = gameObject.transform.localScale;

        activeTime = Time.time;

    }

    void Update() {

        if (shouldAnimate) {

            Animate();

        }

    }

    void Animate() {

        activeTime = activeTime + Time.deltaTime;

        gameObject.transform.localScale = startSize * animationCurve.Evaluate(activeTime);

    }

}
```
