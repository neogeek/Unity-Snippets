# CalculateFrustumPlanes

```csharp
using System.Collections.Generic;
using UnityEngine;

public class SampleController : MonoBehaviour {

    public LayerMask cameraPlanesLayerMask;

    private List<GameObject> cameraPlanes = new List<GameObject>();

    void Awake() {

        Plane[] planes = GeometryUtility.CalculateFrustumPlanes(Camera.main);

        GameObject cameraPlanesWrapper = new GameObject("Camera Planes");

        for (int i = 0; i < planes.Length; i += 1) {

            GameObject plane = GameObject.CreatePrimitive(PrimitiveType.Plane);

            plane.name = string.Format("Plane {0}", i.ToString());
            plane.layer = (int) Mathf.Log(cameraPlanesLayerMask.value, 2);
            plane.transform.parent = cameraPlanesWrapper.transform;

            plane.GetComponent<Renderer>().enabled = false;

            cameraPlanes.Add(plane);

        }

        PositionPlanes();

    }

    void PositionPlanes() {

        Plane[] planes = GeometryUtility.CalculateFrustumPlanes(Camera.main);

        for (int i = 0; i < planes.Length; i += 1) {

            cameraPlanes[i].transform.position = -planes[i].normal * planes[i].distance;
            cameraPlanes[i].transform.rotation = Quaternion.FromToRotation(Vector3.up, planes[i].normal);

        }

    }

}
```
