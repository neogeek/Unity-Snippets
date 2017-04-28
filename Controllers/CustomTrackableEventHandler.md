# CustomTrackableEventHandler.cs

For use with the Unity [Vuforia](https://vuforia.com/) plugin. This is a modified version of an included script that has additional support for enabling and disabling a child game object.

```csharp
using UnityEngine;

namespace Vuforia
{

    public class CustomTrackableEventHandler : MonoBehaviour, ITrackableEventHandler {

        private TrackableBehaviour mTrackableBehaviour;

        public GameObject trackableObj;

        private bool isEnabled = true;

        void Start () {

            mTrackableBehaviour = GetComponent<TrackableBehaviour>();

            if (mTrackableBehaviour) {

                mTrackableBehaviour.RegisterTrackableEventHandler(this);

            }

            OnTrackingLost();

        }

        public void OnTrackableStateChanged (TrackableBehaviour.Status previousStatus, TrackableBehaviour.Status newStatus) {

            if (newStatus == TrackableBehaviour.Status.DETECTED ||
                newStatus == TrackableBehaviour.Status.TRACKED ||
                newStatus == TrackableBehaviour.Status.EXTENDED_TRACKED) {

                OnTrackingFound();

            } else {

                OnTrackingLost();

            }

        }

        void OnTrackingFound () {

            Renderer[] rendererComponents = GetComponentsInChildren<Renderer>(true);

            foreach (Renderer component in rendererComponents) {
                component.enabled = true;
            }

            isEnabled = true;

            if (trackableObj) {

                trackableObj.SetActive(true);

            }

        }

        void OnTrackingLost () {

            Renderer[] rendererComponents = GetComponentsInChildren<Renderer>(true);

            foreach (Renderer component in rendererComponents) {
                component.enabled = false;
            }

            isEnabled = false;

            if (trackableObj) {

                trackableObj.SetActive(false);

            }

        }

    }

}
```
