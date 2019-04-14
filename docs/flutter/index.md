# Flutter

## Was ist Flutter?
[Flutter](https://flutter.dev/) ist ein SDK für Android und iOS Apps. Der größte Vorteil ist, dass die Apps sich eine in [Dart](https://www.dartlang.org/) geschriebene Codebase teilen und dann in jeweiligen nativen Sprachen Java und Swift übersetzt werden ohne dabei die Designsprache des jeweiligen Systems zu verletzen. Das Resultat ist eine performante App mit plattformkonsistentem Design durch verhältnismäßig wenig Programmieraufwand. Flutter besteht aus C / C++, Dart und Skia (2D Grafikengine).

Systemanforderungen: min. iPhone 4S mit iOS 8 oder Android 4.1.x und Gerät mit ARM Prozessor

## Workflow
Nach der Installation von Flutter wird zunächst ein neues Projekt erzeugt. Dieses automatisch erzeugte Projekt bringt bereits eine Testapp mit an der man sich orientieren kann.

```bash
flutter create new_app
```

### iOS
Vorraussetzungen: Mac OSX, Xcode

#### Simulation
```bash
open -a Simulator
flutter run
```

#### USB-Debugging
Zunächst muss man sich mit seiner Apple-ID bei Xcode anmelden und ein iOS-Entwickler Zertifikat generieren. Danach das iPhone an den Mac anschließen und als Zielsystem auswählen und mit `flutter run` den Buildprozess und die Übertragung starten. Danach erscheint die App auf dem Homebildschrim des iPhones.


### Android
Vorraussetzung: Android Studio

#### Simulation
Als erstes erstellt man in Android Studio unter AVD Manager ein virtuelles Gerät und startet dieses. Danach kann man mit `flutter run` die App builden und auf das virtuelle Gerät übertragen.

#### USB-Debugging
Zunächst ist es notwendig USB-Debugging in den Entwickleroptionen am Gerät zu aktivieren. Daraufhin kann das Smartphone per USB an PC oder Mac angeschlossen werden und mit `flutter run` den Buildprozess und die Übertragung starten. Das Zielsystem wird dabei automatisch ausgewählt.


## Aufbau einer Flutter App
**Grundprinzip: Alles ist ein Widget**

Widgets sind die Grundbausteine des gesamten UIs. Jedes Widget liegt innerhalb eines übergeordneten Widgets und erbt dessen Eigenschaften. Sogar die denkbar einfachste App erbt von einem anderen Widget.

```Dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

### Beispiel einer Flutter App

Bei der Beispielapp liegt in dem App-Widget ein Scaffold-Widget und in diesem wiederum App-Bar, Header-Image und List. So wird Schritt für Schritt (bzw. Widget für Widget) das UI aufgebaut.

![Flutter-App](./img/flutter_app.png)
![Flutter-App-Diagram](./img/flutter_app_dia.png)
