# Audio

## Play Audio

```csharp
AudioSource bkgMusic = gameObject.AddComponent<AudioSource>();
bkgMusic.clip = Resources.Load("Audio/BackgroundMusic") as AudioClip;
bkgMusic.Play();
```
