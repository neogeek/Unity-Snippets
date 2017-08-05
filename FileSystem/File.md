# File

## Writing a file to the current directory

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start() {

        System.IO.File.WriteAllText("test.log", "Hello, World.");

    }

}
```
