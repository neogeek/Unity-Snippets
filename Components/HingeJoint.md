# HingeJoint

```csharp
Rigidbody rb = sectionPlatform.GetComponent<Rigidbody>();
rb.isKinematic = false;
rb.angularDrag = 20.0f;

JointLimits limits = new JointLimits();
limits.min = -10.0f;
limits.max = 10.0f;
limits.bounciness = 0.25f;

HingeJoint hinge = sectionPlatform.AddComponent<HingeJoint>();
hinge.axis = new Vector3(0, 0, -1.0f);
hinge.limits = limits;
hinge.useLimits = true;
```
