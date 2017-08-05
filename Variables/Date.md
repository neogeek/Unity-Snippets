# Date

## `DateTime.Now`

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start() {

        Debug.Log(System.DateTime.Now.Date);
        Debug.Log(System.DateTime.Now.TimeOfDay);
        Debug.Log(System.DateTime.Now.ToString("MM/dd/yyyy HH:mm:ss"));

    }

}
```
