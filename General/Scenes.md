# Scenes

## Load Scene

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SampleController : MonoBehaviour {

    void Start() {

        SceneManager.LoadScene(1);

    }

}
```

## Load Scene (Additive)

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SampleController : MonoBehaviour {

    void Start() {

        SceneManager.LoadScene(1, LoadSceneMode.Additive);

    }

}
```
