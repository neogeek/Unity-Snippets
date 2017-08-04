# Lists

## `public` and `private` Class Lists

```csharp
using System.Collections.Generic;
using UnityEngine;

public class SampleController : MonoBehaviour {

    public List<string> publicList;

    private List<string> privateList = new List<string>();

    void Start() {

        publicList.Add("Hello, world");

        privateList.Add("Hello, world");

    }

}
```

**Note:** Pushing to a public uninstantiated list only works because Unity automatically creates one through the inspector panel of whatever game object the script is attached to.
