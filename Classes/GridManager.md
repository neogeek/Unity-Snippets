# GridManager.cs

```csharp
using System.Collections.Generic;
using UnityEngine;

public class GridManager {

    public List<Vector2> gridPositions = new List<Vector2>();

    private int columns = 0;
    private int rows = 0;

    public GridManager(int columnCount = 0, int rowCount = 0) {

        columns = columnCount;
        rows = rowCount;

        Reset();

    }

    public void Reset() {

        gridPositions.Clear();

        for (int x = 0; x < columns; x++) {

            for (int y = 0; y < rows; y++) {

                gridPositions.Add(new Vector2(x, y));

            }

        }

    }

    public Vector2 RandomPosition(bool remove = false) {

        int index = Random.Range(0, gridPositions.Count);

        Vector2 position = gridPositions[index];

        if (remove) {

            gridPositions.RemoveAt(index);

        }

        return position;

    }

}
```
