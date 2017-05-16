# CalculateParentBounds

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public struct LocalBounds {
    public float minX;
    public float maxX;
    public float minY;
    public float maxY;
    public float minZ;
    public float maxZ;
}
```

```csharp
LocalBounds CalculateParentBounds (GameObject parentObj) {

    LocalBounds caluclatedBounds = new LocalBounds();

    caluclatedBounds.minX = 0;
    caluclatedBounds.maxX = 0;

    caluclatedBounds.minY = 0;
    caluclatedBounds.maxY = 0;

    for (int i = 0; i < parentObj.transform.childCount; i++) {

        Bounds currentBounds = parentObj.transform.GetChild(i).GetComponent<Renderer>().bounds;

        if (currentBounds.min.x < caluclatedBounds.minX) {

            caluclatedBounds.minX = currentBounds.min.x;

        } else if (currentBounds.max.x > caluclatedBounds.maxX) {

            caluclatedBounds.maxX = currentBounds.max.x;

        }

    }

    return caluclatedBounds;

}
```
