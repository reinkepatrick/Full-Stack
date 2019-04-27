## Was ist React?

[React](https://reactjs.org/) ist ein JavaScript Framework, von
Facebook, für die Erstellung von Benutzeroberflächen. Es ist eins der
größten JavaScript Frameworks neben Angular und Vue.js. React steht
aktuell bei der Version 16.8.6 vom 27. März 2019, die neusten Neuerungen
sind die sogenannten Hooks welche später genauer erläutert werden.

## Anforderungen

React bietet große Möglichkeiten SPA (_Singe Page Application_) und MPA
(_Multi Page Application_) zu erstellen, aber um mit React einem guten
Workflow nachgehen zu können benötigt man etwas Vorwissen. Man sollte
wissen wie sich der neue JavaScript Standard (_ES6_) verhält und was
genau der Unterschied zwischen SPA und MPA ist.

## Workflow

Die Dependencies spielen eine große Rolle in der heutigen Zeit von
JavaScript um dort den Überblick und den bestmöglichen Komfort zu haben
benutzen wir einen sogenanntes Dependency Managment Tool. In unserem
Fall nutzen wir den Node Package Manager (_npm_). Wir nutzen einen
sogenannten Bundler der aus all unseren Dateien ein kompaktes Produkt
formt, hier zu nutzen wir Webpack. Um wie oben, in den
[Anforderungen](#anforderungen) angesprochen, ES6 verwenden zu können
benötigen wir einen Compiler, dieser sorgt dafür das aus ES6 gängiges
JavaScript wird und es für jeden Browser verständlich ist. Diese Arbeit
nimmt uns bei React ein Tool ab, dieses Tool nennt sich
[create-react-app](https://github.com/facebook/create-react-app).

```bash
npm install -g create-react-app
create-react-app test-react-app
```

Um die App im Browser richtig darstellen zu können starten wir den
mitgebrachten Webserver über npm, die App ist dann über
[localhost:3000](http://localhost:3000) erreichbar. __Bei einer Änderung
an unserer App wird diese automatisch neu erstellt.__

```bash
npm start
```
