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

## Das Herzstück

Das Herzstück einer Angular App ist standartmäßig die Datei `app.module.ts`, diese Datei kann jedoch auch umbenannt werden. Dort laufen alle Funktionalitäten auf unterschiedliche Weise zusammen. Alle Dateien werden über Imports eingebunden.

Beim Erstellen einer neuen Angular App über die Angular CLI werden in der `app.module.ts` zunächst die wichtigsten Module geladen.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**BrowserModule** dient dazu, die Anwendung zu starten.

**NgModule** stellt das Hauptmodul der Anwendung dar und erstellt das App-Modul. Beim Laden dieses Moduls wird der *AppComponent* gestartet, wie unter `bootstrap` angegeben.

**AppRoutingModule** ist optional und bietet die Möglichkeit, Inhalt in Abhängigkeit der aktuellen URL zu laden. Der Import dieser Datei kann schon während der Einrichtung über die Angular CLI erfolgen. Siehe Abschnitt *Projekt erstellen*.

**AppComponent** ist die Elternkomponente, welche alle weiteren App-Komponenten als Kind-Komponente enthält.

## Komponenten

Angular ist komponentenorientiert, die gesamte Anwendung ist zunächst eine Komponente, auch Top Level Komponente genannt. Darunter sind wieder weitere Komponenten die selbst auch wieder Komponenten beinhalten könnten. Eine Anwendung setzt sich somit aus vielen Komponenten zusammen. So existiert zum Beispiel für die Navigation, einzelne Unterseiten usw. eigene Komponenten.

![Angular Komententenbaum](img\components_dia.png)

Eine Komponenten besteht im wesentlichen aus 2 Teilen. Dem *Component Controller* welcher die Logik beinhaltet sowie einem *Template* welches für die Darstellung verantwortlich ist und üblicherweise aus HTML besteht. Des Weiteren gibt es eine CSS Datei welche nur für die entsprechende Komponente gilt.

Der Inhalt der *AppComponent* sieht wie folgt aus:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Full-Stack';
}
```

Unter **selector** wird angegeben, wie dieses Modul eingebunden werden soll (HTML-Element oder HTML-Attribut). Die **templateUrl** und **styleUrls** beinhalten Pfade zum Template bzw. Stylesheet dieser Komponente. Es ist außerdem möglich, anstelle von Pfaden Inline Code zu verwenden. Dafür muss jedoch *templateUrl* in *template* und *styleUrls* in *styles* umgeschrieben werden.