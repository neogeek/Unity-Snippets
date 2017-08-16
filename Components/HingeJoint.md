# HingeJoint

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    void Start() {

        Rigidbody rb = gameObject.AddComponent<Rigidbody>();
        rb.isKinematic = false;
        rb.angularDrag = 20.0f;

        JointLimits limits = new JointLimits();
        limits.min = -10.0f;
        limits.max = 10.0f;
        limits.bounciness = 0.5f;

        HingeJoint hinge = gameObject.AddComponent<HingeJoint>();
        hinge.axis = new Vector3(0, 0, -1.0f);
        hinge.limits = limits;
        hinge.useLimits = true;

    }

}
```
