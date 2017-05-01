# GridManager.cs

```csharp
using System.Collections.Generic;
using UnityEngine;

public class GridManager {

    public int columns = 0;
    public int rows = 0;

    public List<Vector3> gridPositions = new List<Vector3>();

    public GridManager (int columnCount, int rowCount) {

        columns = columnCount;
        rows = rowCount;

        Reset();

    }

    public void Reset () {

        gridPositions.Clear();

        for (int x = 0; x < columns; x++) {

            for (int y = 0; y < rows; y++) {

                gridPositions.Add(new Vector2(x, y));

            }

        }

    }

    public Vector2 RandomPosition () {

        int randomIndex = (int)Random.Range(0, gridPositions.Count);

        Vector2 position = gridPositions[randomIndex];

        gridPositions.RemoveAt(randomIndex);

        return position;

    }

}
```
