# Singleton

**Singleton.cs**

```csharp
using UnityEngine;

public class Singleton : MonoBehaviour {

    private static Singleton _instance;

    public static Singleton instance {

        get { return _instance; }

    }

    private void Awake () {

        if (_instance != null && _instance != this) {

            Destroy(gameObject);

        } else {

            _instance = this;

            DontDestroyOnLoad(gameObject);

        }

    }

    private void OnDestroy() {

        if (_instance == this) {

            _instance = null;

        }

    }

}
```

**SampleController.cs**

```csharp
using UnityEngine;

public class SampleController : Singleton {

}
```
