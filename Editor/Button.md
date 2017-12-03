# Button

![](https://i.imgur.com/VVXeotA.png)

```csharp
using UnityEditor;
using UnityEngine;

public class SampleController : MonoBehaviour {

    public void TestMethod() {

        Debug.Log("test");

    }

}

[CustomEditor(typeof(SampleController))]
public class SampleControllerEditor : Editor {

    public override void OnInspectorGUI() {

        DrawDefaultInspector();

        SampleController script = (SampleController) target;

        if (GUILayout.Button("Test Button")) {

            script.TestMethod();

        }

    }

}
```
