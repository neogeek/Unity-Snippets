# Fire TV

## Connect

```bash
$ adb kill-server
$ adb start-server
$ adb connect <ipaddress>
```

## Install

```bash
$ adb install ~/Desktop/HelloWorld.apk
```

## Start Game From Command Line

```bash
$ adb shell am start -S com.amazon.sample.helloworld/com.unity3d.player.UnityPlayerNativeActivity
```

## Reference

- <https://developer.amazon.com/docs/fire-tv/connecting-adb-to-device.html>
- <https://developer.amazon.com/docs/fire-tv/installing-and-running-your-app.html>
