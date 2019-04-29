<img src="img/angular_logo.png" width="300px" height="300px" />



# Angular

## Kurzbeschreibung

Ermöglicht effizientere und leichtere Kommunikation zwischen HTML und JavaScript. Angular ist ein Open-Source-Framework zur Erstellung von browserbasierten Single-Page-Anwendungen und Web-Apps. Durch die Nutzung des Model View Controllers (MVC-Modell) wird die Softwareentwicklung und das Modultesten von Apps verbessert. Angular strukturiert sowohl den App-Aufbau als auch die Entwicklung effizienter und erleichtert die Kommunikation zwischen HTML und JavaScript.

## Erste Schritte

### Installation

Die Entwicklung mit Angular setzt folgende Abhängigkeiten voraus

1. Node.js

2. npm

Nachdem Node.js und npm installiert wurden, kann das Angular Projekt durch die Angular CLI  erstellt werden. Dafür ist es notwendig, diese zunächst durch 

```shell
> npm install @angular/cli -g
```

per npm zu installieren.

Es ist stets sinnvoll die aktuellen Version zu nutzen. Diese kann jeweils mit 

```shell
> npm --version
```

```shell
> node --version
```

```shell
> ng --version
```

 geprüft werden.

### Projekt erstellen

Um ein Angular Projekt zu erstellen, wechselt man zunächst in das gewünschte Zielverzeichnis. Anschließend wird per Angular CLI ein neues Projekt erstellt.

```shell
> ng new <Name_der_Applikation>
```

Im darauf folgenden Dialog können nun folgende zwei Einstellungen getroffen werden.

```shell
Would you like to add Angular routing? (y/N)
```

Beantwortet man mit `y`, stellt einem die Angular CLI ein Routing Gerüst zur Verfügung. Im nächstem Dialog kann daraufhin der gewünschte CSS-Präprozessor gewählt werden.

```
Which stylesheet format would you like to use?
```

Im Anschluss wird die Projektstruktur erstellt. Wechselt man nun in das neu erstellte Verzeichnis, kann mit

```shell
ng serve
```

das Angular Projekt gestartet werden. Dieser Vorgang kann eine Weile dauern, da unter anderem TypeScript in JavaScript kompiliert werden muss. Die Standard Website kann nun unter `localhost:4200` im Browser aufgerufen werden.

![Angular first serve](img/first_serve.png)



