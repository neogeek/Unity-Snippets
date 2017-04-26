# `yield`

```csharp
using System.Collections;
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start () {

        StartCoroutine(logOutput());

    }

    IEnumerator logOutput () {

        yield return new WaitForSeconds(1);

        StartCoroutine(logOutput());

    }

}
```
