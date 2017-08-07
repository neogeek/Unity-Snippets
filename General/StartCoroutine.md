# `StartCoroutine`

```csharp
using System.Collections;
using UnityEngine;

public class SampleController : MonoBehaviour {

    private readonly WaitForSeconds delay = new WaitForSeconds(1);

    private IEnumerator coroutine;

    void Start() {

        coroutine = logOutput("output");

        StartCoroutine(coroutine);

    }

    IEnumerator logOutput(string output) {

        yield return delay;

        Debug.Log(output);

        coroutine = logOutput("output");

        StartCoroutine(coroutine);

    }

}
```
