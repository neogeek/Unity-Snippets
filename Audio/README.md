# Audio

## Play Audio

```csharp
AudioSource bkgMusic = gameObject.AddComponent<AudioSource>();
bkgMusic.clip = Resources.Load("BackgroundMusic") as AudioClip;
bkgMusic.Play();
```

**Note:** Only one audio source can play at a time on any given game object.
