# FadeController

```csharp
public class SampleController : MonoBehaviour {

    public FadeController fader;

    void Update () {

        if (Input.GetKeyDown(KeyCode.Space) || (Input.touchCount > 0 && Input.GetTouch(0).phase == TouchPhase.Began)) {

            fader.FadeOut();

        }

        if (fader.HasCompleted()) {

            SceneManager.LoadScene(1);

        }

    }

}
```

```csharp
using UnityEngine;

public class FadeController : MonoBehaviour {

    private Texture2D fadeTexture;
    private Color fadeColor;
    private float fadeSpeed = 0.3f;

    private bool isFading = false;
    private bool isDoneFading = false;
    private float fadeDirection = 1.0f;

    void Awake () {

        fadeTexture = new Texture2D(0, 0);
        fadeColor = new Color(0, 0, 0, 0.0f);

    }

    public void FadeOut () {

        isFading = true;
        Time.timeScale = 0;
        fadeDirection = 1.0f;
        fadeColor.a = 0.0f;

    }

    public void FadeIn () {

        isFading = true;
        Time.timeScale = 0;
        fadeDirection = -1.0f;
        fadeColor.a = 1.0f;

    }

    public bool HasCompleted () {

        return isDoneFading;

    }

    void Update () {

        if (isFading) {

            if (fadeColor.a != fadeDirection) {

                fadeColor.a = Mathf.Clamp01(fadeColor.a + (Time.unscaledDeltaTime / fadeSpeed * fadeDirection));

            } else {

                Time.timeScale = 1;
                isFading = false;
                isDoneFading = true;

            }

        }

    }

    void OnGUI () {

        if (isFading || isDoneFading) {

            GUI.color = fadeColor;
            GUI.DrawTexture(new Rect(0, 0, Screen.width, Screen.height), fadeTexture);

        }

    }

}
```
