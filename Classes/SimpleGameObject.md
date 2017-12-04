# SimpleGameObject

*SimpleGameObject.cs*

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

namespace SGO {

    [System.Serializable]
    public struct Transform {
        public Quaternion localRotation;
        public Vector3 localScale;
        public Vector3 localPosition;
        public Transform(UnityEngine.Transform transform) {
            localRotation = transform.localRotation;
            localScale = transform.localScale;
            localPosition = transform.localPosition;
        }
    }

    [System.Serializable]
    public struct BoxCollider {
        public bool isTrigger;
        public UnityEngine.Vector3 center;
        public UnityEngine.Vector3 size;
        public BoxCollider(UnityEngine.Collider boxCollider) {
            isTrigger = boxCollider.isTrigger;
            center = boxCollider.bounds.center;
            size = boxCollider.bounds.size;
        }
    }

    [System.Serializable]
    public struct GameObject {
        public SGO.Transform transform;
        public SGO.BoxCollider boxCollider;
    }

    [System.Serializable]
    public struct GameObjectList {
        public GameObject[] gameObjects;
    }

    public class SimpleGameObject {

        public static string ExportToJSON(UnityEngine.GameObject[] gameObjects) {

            SGO.GameObjectList list = new SGO.GameObjectList();

            list.gameObjects = new SGO.GameObject[gameObjects.Length];

            for (int i = 0; i < gameObjects.Length; i = i + 1) {

                SGO.GameObject simpleGameObject = new SGO.GameObject();

                simpleGameObject.transform = new SGO.Transform(gameObjects[i].transform);
                simpleGameObject.boxCollider = new SGO.BoxCollider(gameObjects[i].GetComponent<UnityEngine.BoxCollider>());

                list.gameObjects[i] = simpleGameObject;

            }

            return JsonUtility.ToJson(list);

        }

        public static SGO.GameObjectList ImportFromJSON(string fileContents) {

            return JsonUtility.FromJson<SGO.GameObjectList>(fileContents);

        }

    }

}
```

*SerializeObjects.cs*

```csharp
using UnityEditor;
using UnityEngine;

public class SerializeObjects : MonoBehaviour {

    public string tagName = "Exportable";
    public string fileName = "Assets/test.json";

    public SGO.GameObjectList list;

    public void ImportFromJSON() {

        list = SGO.SimpleGameObject.ImportFromJSON((Resources.Load(fileName) as TextAsset).text);

        Debug.Log(list);

    }

    public void ExportToJSON() {

        string output = SGO.SimpleGameObject.ExportToJSON(GameObject.FindGameObjectsWithTag(tagName));

        System.IO.File.WriteAllText(fileName, output);

        Debug.Log(output);

    }

}

[CustomEditor(typeof(SerializeObjects))]
public class SerializeObjectsEditor : Editor {

    public override void OnInspectorGUI() {

        DrawDefaultInspector();

        SerializeObjects script = (SerializeObjects) target;

        if (GUILayout.Button("Import From JSON")) {

            script.ImportFromJSON();

        }

        if (GUILayout.Button("Export To JSON")) {

            script.ExportToJSON();

        }

    }

}
```
