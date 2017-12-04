# SimpleGameObject

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
        public UnityEngine.Transform transform;
        public UnityEngine.BoxCollider boxCollider;
    }

    [System.Serializable]
    public struct GameObjectList {
        public GameObject[] gameObjects;
    }

}
```
