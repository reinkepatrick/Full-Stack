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
JavaScript um dort den Überblick und den bestmöglichen Komfort zu bieten
wird einen sogenanntes Dependency Management Tool empfohlen. In diesem
Fall wird der Node Package Manager (_npm_) genutzt. Es wird ein Bundler,
der aus allen Dateien ein kompaktes Produkt formt, genutzt. React
verwendet dafür Webpack. Um wie oben, in den
[Anforderungen](#anforderungen) angesprochen, ES6 verwenden zu können
wird ein Compiler genutzt, dieser sorgt dafür das aus ES6 gängiges
JavaScript wird das für jeden modernen Browser verständlich ist. Diese
Arbeit nimmt bei React ein Tool ab, dieses Tool nennt sich
[create-react-app](https://github.com/facebook/create-react-app).

```bash
npm install -g create-react-app
create-react-app test-react-app
```

Um die App im Browser richtig darstellen zu können wird der
mitgebrachten Webserver über npm gestartet, die App ist dann über
[localhost:3000](http://localhost:3000) erreichbar. __Bei einer Änderung
an der App wird diese automatisch neu erstellt.__

```bash
npm start
```

