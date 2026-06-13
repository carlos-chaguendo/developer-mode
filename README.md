# Developer Mode

## Herramientas


* [https://lookin.work](https://lookin.work)
* [https://www.rocketsim.app](https://www.rocketsim.app)
* [Faster Xcode Download ](https://github.com/vineetchoudhary/Downloader-for-Apple-Developers)
	* [https://xcodereleases.com/](https://xcodereleases.com/) 
* [Macdown](https://macdown.uranusjr.com) is an open source Markdown editor for macOS
* [CaptuOCR](https://github.com/carlos-chaguendo/capture-ocr) Take a screenshot and extract text

## Configuraciones
* [XCode - The Build Process](https://www.objc.io/issues/6-build-tools/build-process/)

## Documentación

* [Recursos Recomendados](recursos.md) :ok_hand:
* [SwiftUI](swift-ui.md)
* [SwiftUI Metal Shader](https://thebookofshaders.com/00/?lan=es)

## Comandos


```
// Apps instaladas
xcrun simctl listapps booted

// Reiniciar los permisos de ubicacion
xcrun simctl privacy booted reset location com.apple.Maps

// Matar procesos del simulador colgados
sudo killall -9 com.apple.CoreSimulator.CoreSimulatorService
xcrun simctl shutdown all

// Revokar permisos para que claude code controle otras apps
tccutil reset AppleEvents com.anthropic.claude-code

// Activar el panel de subtitulos de VoiceOver
xcrun simctl spawn <device-id> defaults write com.apple.VoiceOverTouch VoiceOverCaptionPanelEnabled -bool true
xcrun simctl spawn <device-id> defaults domains

```

##  Debugging Magic 
```
https://developer.apple.com/library/archive/technotes/tn2124/_index.html#//apple_ref/doc/uid/DTS10003391-CH1-SECWEBSERVICES
https://github.com/armcknight/Awesome-Xcode-Debugging
https://suelan.github.io/2020/02/05/iOS-Simulator-from-the-Command-Line/
```

```sh
Con el crash report. Pasos:

  1. xcrun simctl install colgaba sin error visible
  2. Busqué crash reports en ~/Library/Logs/DiagnosticReports/ — había varios CoreSimulatorBridge-*.ips generados exactamente durante los intentos fallidos
  3. Leí el más reciente — en el campo termination.reasons decía explícitamente:
  Library not loaded: /usr/lib/swift/libswiftXPC.dylib
  tried: '.../iOS 15.4.simruntime/.../libswiftXPC.dylib' (no such file)
  4. Busqué esa librería con find en todos los runtimes — solo existía en iOS 16.1
  5. Copié de iOS 16.1 a iOS 15.x


  SRC="/Library/Developer/CoreSimulator/Volumes/iOS_20B72/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS 16.1.simruntime/Contents/Resources/RuntimeRoot/usr/lib/swift/libswiftXPC.dylib"

  sudo cp "$SRC" "/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS 15.5.simruntime/Contents/Resources/RuntimeRoot/usr/lib/swift/libswiftXPC.dylib"
  sudo cp "$SRC" "/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS 15.4.simruntime/Contents/Resources/RuntimeRoot/usr/lib/swift/libswiftXPC.dylib"
```

## Private Frameworks

- Invocar fameworks privados desde swift https://medium.com/@victor.pavlychko/private-apis-objective-c-runtime-and-swift-ceaeefbb6e48
