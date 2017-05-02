# Singleton

**SampleSingleton.cs**

```csharp
using UnityEngine;

public class SampleSingleton : MonoBehaviour {

    public static SampleSingleton instance = null;

    void Awake () {

        if (instance != null && instance != this) {

            Destroy(gameObject);

            return;

        }

        instance = this;

        DontDestroyOnLoad(gameObject);

    }

}
```

**Loader.cs**

```csharp
using UnityEngine;

public class Loader : MonoBehaviour {

    public GameObject sampleSingleton;

    void Awake () {

        if (SampleSingleton.instance == null) {

            Instantiate(sampleSingleton);

        }

    }

}
```
