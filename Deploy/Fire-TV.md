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

- <https://developer.amazon.com/public/solutions/devices/fire-tv/docs/connecting-adb>
- <https://developer.amazon.com/public/solutions/devices/fire-tv/docs/installing-and-running-your-app>
