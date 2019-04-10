# React

## Anforderungen
React bietet große Möglichkeiten SPA (_Singe Page Application_) und 
MPA (_Multi Page Application_) zu erstellen, aber um mit React einem guten Workflow 
nachgehen zu können benötigt man etwas Vorwissen. Man sollte wissen wie sich der neue 
JavaScript Standard (_ES6_) verhält und was genau der Unterschied zwischen SPA und MPA ist.

## Workflow
Die Dependencies spielen eine große Rolle in der heutigen Zeit von JavaScript um dort
den Überblick und den bestmöglichen Komfort zu haben benutzen wir einen sogenanntes
Dependency Managment Tool. In unserem Fall nutzen wir den Node Package Manager (_npm_).
Wir nutzen einen sogenannten Bundler der aus all unseren Dateien ein kompaktes Produkt 
formt, hier zu nutzen wir Webpack. Um wie oben, in den 
[Anforderungen](#anforderungen) angesprochen, ES6 verwenden zu können benötigen wir
einen Compiler, dieser sorgt dafür das aus ES6 gängiges JavaScript wird und es für jeden
Browser verständlich ist. Diese Arbeit nimmt uns bei React ein Tool ab, dieses Tool
nennt sich [create-react-app](https://github.com/facebook/create-react-app).

```bash
npm install -g create-react-app
create-react-app test-react-app
```

Um die App im Browser richtig darstellen zu können starten wir den mitgebrachten 
Webserver über npm, die App ist dann über [localhost:3000](http://localhost:3000) 
erreichbar. __Bei einer Änderung an unserer App wird diese automatisch neu erstellt.__

```bash
npm start
```

## Komponenten 
Komponenten sind das grundlegendste in React, aus einzelnen Komponenten wird unsere App
am Ende zusammengesetzt. Der Vorteil an Komponenten sind das man sie immer wieder 
verwenden kann. Der allgemeine Aufbau eines Komponent (_src/App.js_) in React sieht wie folgt aus:

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>Hello World</h1>
      </div>
    );
  }
}

export default App;
```

## JSX
JSX ist die Sprache die zum darstellen von unseren Komponenten verwenden, um genauer zu
sagen den Teil den wir die `render()` Funktion schreiben.

```js
render() {
 return (
   <div className="App">
     <h1>Hello World</h1>
   </div>
 );
}
```

Damit wir dort JSX verwenden können importieren wir auch `React` in unserem Komponent.
React erstellt dann aus dem JSX für den Browser lesbares JavaScript. Als Beispiel zeige
zeige ich euch wie der übere Code als normales JavaScript aussieht und was React intern
mit JSX macht.

```js
render() {
 return React.createElement('div', 
                            { className: 'App' }, 
                            React.createElement('h1', null, 'Hello World'));
}
```

JSX hat aber ein paar kleine Einschränkungen, es sieht zwar aus wie HTML und es verhält 
sich auch in den meisten Fällen so, weswegen wir auch zu Beispiel `className` nutzen 
anstatt die normale HTML-`class`, weil JSX intern immer noch zu JavaScrip kompiliert wird
und `class` unter JavaScript eine andere Verwendung findet. Außerdem kann man unter JSX
nur ein HTML Element zurückgeben, am folgenden Beispiel sieht man wie es __nicht__ 
funktioniert.

```js
render() {
 return (
   <div className="App">
     <h1>Hello World</h1>
   </div>
   <p>I'm not working</>
 );
}
```

Aus diesem Grund packt man um ein Komponent immer ein HTML-Element und fügt sein Kontent
in dieses HTML-Element.

