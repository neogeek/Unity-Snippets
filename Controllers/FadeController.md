# FadeController

```csharp
using UnityEngine;

public class FadeController : MonoBehaviour {

    public Color fadeColor = new Color(0, 0, 0, 0.0f);
    public float fadeSpeed = 0.3f;

    public bool isFading {
        get {
            return fadeColor.a != fadeEndValue;
        }
    }

    private Texture2D fadeTexture;
    private Rect fadeRect;

    private float fadeDirection = -1.0f;
    private float fadeEndValue = 0.0f;

    void Awake() {

        fadeTexture = new Texture2D(0, 0);
        fadeRect = new Rect(0, 0, Screen.width, Screen.height);

    }

    public void FadeOut() {

        fadeDirection = 1.0f;
        fadeEndValue = 1.0f;

    }

    public void FadeIn() {

        fadeDirection = -1.0f;
        fadeEndValue = 0.0f;

    }

    void Update() {

        if (isFading) {

            fadeColor.a = Mathf.Clamp01(fadeColor.a + (Time.fixedDeltaTime / fadeSpeed * fadeDirection));

        }

    }

    void OnGUI() {

        GUI.color = fadeColor;
        GUI.DrawTexture(fadeRect, fadeTexture);

    }

}
```

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SampleController : MonoBehaviour {

    public FadeController fader;

    private bool fadeStarted = false;

    void Update() {

        if (Input.GetKeyDown(KeyCode.Space) || (Input.touchCount > 0 && Input.GetTouch(0).phase == TouchPhase.Began)) {

            if (!fadeStarted) {

                fader.FadeOut();

                fadeStarted = true;

            }

        }

        if (fadeStarted && !fader.isFading) {

            SceneManager.LoadScene(0);

        }

    }

}
```
