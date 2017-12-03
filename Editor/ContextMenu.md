# ContextMenu

![](https://i.imgur.com/3m8xCOZ.png)

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    public int randomNumber = 0;

    [ContextMenu("Generate Random Number")]
    void GenerateRandomNumber() {

        randomNumber = Random.Range(1, 10);

    }

}
```
