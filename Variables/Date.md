# Date

## `DateTime.Now`

```csharp
using System;
using UnityEngine;

public class DateController : MonoBehaviour {

    void Start () {

        Debug.Log(System.DateTime.Now.Date);
        Debug.Log(System.DateTime.Now.TimeOfDay);
        Debug.Log(System.DateTime.Now.ToString("MM/dd/yyyy HH:mm:ss"));

    }

}
```
