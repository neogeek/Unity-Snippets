# CustomTrackableEventHandler.cs

For use with the Unity [Vuforia](https://vuforia.com/) plugin. This is a modified version of an included script that has additional support for enabling and disabling a child game object.

```csharp
using UnityEngine;

namespace Vuforia {

    public class CustomTrackableEventHandler : MonoBehaviour, ITrackableEventHandler {

        public GameObject trackableObj;

        private TrackableBehaviour mTrackableBehaviour;

        void Start() {

            mTrackableBehaviour = gameObject.GetComponent<TrackableBehaviour>();

            if (mTrackableBehaviour) {

                mTrackableBehaviour.RegisterTrackableEventHandler(this);

            }

            OnTrackingLost();

        }

        public void OnTrackableStateChanged(TrackableBehaviour.Status previousStatus, TrackableBehaviour.Status newStatus) {

            if (newStatus == TrackableBehaviour.Status.DETECTED ||
                newStatus == TrackableBehaviour.Status.TRACKED ||
                newStatus == TrackableBehaviour.Status.EXTENDED_TRACKED) {

                OnTrackingFound();

            } else {

                OnTrackingLost();

            }

        }

        void OnTrackingFound() {

            if (trackableObj && !trackableObj.activeSelf) {

                trackableObj.SetActive(true);

            }

        }

        void OnTrackingLost() {

            if (trackableObj && trackableObj.activeSelf) {

                trackableObj.SetActive(false);

            }

        }

    }

}
```
