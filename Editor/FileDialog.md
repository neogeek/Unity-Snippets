# File Dialog

```csharp
using System.IO;
using UnityEditor;
using UnityEngine;

public class SampleController : MonoBehaviour {

    [ContextMenu("Select File")]
    void SelectFile() {

        string path = EditorUtility.OpenFilePanelWithFilters("Select File", "Assets/", new string[] { "PNG", "png" });

        if (path.Length != 0) {

            string[] files = Directory.GetFiles(path);

            Debug.Log(files[0]);

        }

    }

}
```
