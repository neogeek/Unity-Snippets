# Singleton

**SampleSingleton.cs**

```csharp
using UnityEngine;

public class SampleSingleton : MonoBehaviour {

    public static SampleSingleton instance = null;

    public int highScore = 0;
    public int currentScore = 0;

    void Awake () {

        if (instance != null && instance != this) {

            Destroy(gameObject);

            return;

        }

        instance = this;

        DontDestroyOnLoad(gameObject);

    }

    public void SceneSetup () {

        currentScore = 0;

    }

    void Update () {

        currentScore = currentScore + 1;

        if (currentScore > highScore) {

            highScore = currentScore;

        }

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

        SampleSingleton.instance.SceneSetup();

    }

}
```
