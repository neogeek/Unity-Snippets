# DestroyParticleEmitterOnEnd

```csharp
using UnityEngine;

public class DestroyParticleEmitterOnEnd : MonoBehaviour {

    private ParticleSystem ps;

    void Awake () {

        ps = gameObject.GetComponent<ParticleSystem>();

    }

    void Update () {

        if (!ps.IsAlive()) {

            Destroy(gameObject);

        }

    }

}
```
