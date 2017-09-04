# GUI

```csharp
#if UNITY_EDITOR

void OnGUI() {

    GUIStyle style = new GUIStyle();
    style.fontSize = 100;
    style.normal.textColor = Color.white;

    GUI.Label(new Rect(20, 20, Screen.width, style.fontSize), score.ToString(), style);

}

#endif
```
