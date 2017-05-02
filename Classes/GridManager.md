# GridManager.cs

```csharp
using System.Collections.Generic;
using UnityEngine;

public class GridManager {

    public int columns = 0;
    public int rows = 0;

    public List<Vector2> gridPositions = new List<Vector2>();

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

    public Vector2 RandomPosition (bool remove = true) {

        return ReturnAndRemove(Random.Range(0, gridPositions.Count), remove);

    }

    public Vector2 FindByPosition (Vector2 position, bool remove = true) {

        return ReturnAndRemove(gridPositions.IndexOf(position), remove);

    }

    public Vector2 ReturnAndRemove (int index, bool remove = true) {

        Vector2 position = gridPositions[index];

        if (remove) {

            gridPositions.RemoveAt(index);

        }

        return position;

    }

}
```
