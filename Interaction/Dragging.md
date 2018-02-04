# Dragging

## Drag GameObject From Center

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour
{

    public LayerMask layerMask = ~0;

    private Camera mainCamera;

    private Transform dragObjectTransform;
    private float dragDistance = 0;
    private Vector3 dragOffset = Vector3.zero;

    private void Awake()
    {

        mainCamera = Camera.main;

    }

    private void Update()
    {

        if (Input.GetMouseButtonDown(0))
        {

            RaycastHit hit;

            if (Physics.Raycast(mainCamera.ScreenPointToRay(Input.mousePosition), out hit, Mathf.Infinity, layerMask))
            {

                dragObjectTransform = hit.transform;
                dragDistance = hit.distance;
                dragOffset = dragObjectTransform.transform.position - hit.point;

            }

        }
        else if (Input.GetMouseButtonUp(0))
        {

            dragObjectTransform = null;
            dragDistance = 0;
            dragOffset = Vector3.zero;

        }

        if (dragObjectTransform != null && Input.GetMouseButton(0))
        {

            dragObjectTransform.position = mainCamera.ScreenPointToRay(Input.mousePosition).GetPoint(dragDistance) + dragOffset;

        }

    }

}
```
