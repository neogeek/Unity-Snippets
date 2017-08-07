# `yield`

```csharp
using System.Collections;
using UnityEngine;

public class SampleController : MonoBehaviour {

    private readonly WaitForSeconds delay = new WaitForSeconds(1);

    void Start() {

        StartCoroutine(logOutput());

    }

    IEnumerator logOutput() {

        yield return delay;

        Debug.Log("output");

        StartCoroutine(logOutput());

    }

}
```
