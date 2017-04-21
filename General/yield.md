# `yield`

```csharp
using System.Collections;
using UnityEngine;

public class ExampleScript : MonoBehaviour {

    void Start () {

        StartCoroutine(logOutput());

    }

    IEnumerator logOutput () {

        yield return new WaitForSeconds(1);

        StartCoroutine(logOutput());

    }

}
```
