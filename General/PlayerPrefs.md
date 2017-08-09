# PlayerPrefs

## Store and Retrieve Player Data/Preferences

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    public int highScore {

        get {
            return PlayerPrefs.GetInt ("highScore", 0);
        }

        set {
            PlayerPrefs.SetInt ("highScore", value);
        }

    }

}
```
