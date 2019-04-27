## Styling

Wie unter normalen HTML gibt es auch hier die Möglichkeit Inline Styling
oder via externe `.css`-File Stylings vorzunehmen. Als erstes schauen
wir uns das Styling via `.css`-File an. Hier für müssen wir eigentlich
nur die `.css`-File importieren.

```jsx
import React from 'react';
```

Nun können wir einfach die Klassen via `className` verwenden. __Wichtig
hier bei ist das alle Änderungen in der `.css`-File global sind.__ Für
das Inline Styling müssen wir unsere Anweisungen in JavaScript
verfassen, in dem nächsten Beispiel speichern wir diese in einer
Variable.

```jsx
const style = {
  backgroundColor: 'white',
  border: '1px solid blue'
};
```

Jetzt müssen wir dies nur noch in unserem JSX Code verwenden.

```jsx
return (
  <div className="App">
    <h1 style={ style }>Hi, I'm a React App</h1>
  </div>
);
```

### Radium

Radium ist eine Reihe an Tools die uns die Möglichkeit geben unsere
Inline Styles von unseren React Components zu verbessern. Diese bauen
außerdem auch Pseudo Selektoren, Media Queries und vieles mehr als
Inline Styling ein.

```bash
npm install radium --save
```

Um es aber nun auch verwenden zu können müssen wir es in der Datei, wo
wir es verwenden wollen, noch einbinden und unser Component beim
zurückgeben darin einpacken (_wrapping_).

```jsx
import Radium from 'radium';
```

```jsx
export default Radium(App);
```

Nun können wir Pseudo Selektoren in unserem Inline Styling verwenden.
Alle Pseudo Selektoren sind unterstützt.

```jsx
const style = {
  backgroundColor: 'white',
  border: '1px solid blue',
  'hover': {
    backgroundColor: 'black',
    color: 'white'
  }
};
```

Als nächstes schauen wir uns an wie man Media Queries unter Radium und
React macht, als Inline Styling.

```jsx
const style = {
  '@media (min-width: 500px)': {
      width: '450px'
  }
};
```

Da wir Media Queries verwenden müssen wir unsere App in ein `StyleRoot`
packen, dasselbe gilt auch für Keyframes.

```jsx
return (
  <StyleRoot>
    <div className="App">
      { helloWorld }
    </div>
  </StyleRoot>
);
```

### Aphrodite

[Aphrodite](https://github.com/Khan/aphrodite) ist eine weitere
Bibliothek für React, die es einem ermöglicht Inline-Style mit
Pseudo-Selektoren und Media Queries zu verwenden. Der größte Unterschied
zu Radium ist, dass es unter Aphrodite eine Möglichkeit gibt
`@font-face` zu verwenden, das man seine Style-Objekte bei der
`StyleSheet.create()`-Funktion übergibt und das man diese in an
`className` übergibt anstatt an `style`, über die Funktion `css`.
Ebenfalls ist es eine Sache von Aphrodite, dass alle Styling bekommen
das `!important`-Tag, dafür kann man aber `aphroidte/no-important`
importieren.

Anhand eines Beispiel lässt sich das besser verdeutlichen:

```jsx
import { StyleSheet, css } from 'aphrodite';

const styles = StyleSheet.create({
                 panel: {
                   backgroundColor: '#00ffff',
                   ':hover': {
                     color: '#ffffff',
                     cursor: 'pointer'
                   },
                   '@media (max-width: 700px)': {
                     backgroundColor: '#ff0000'
                   }
                 }
               });

class App extends Component {
  render() {
    return (
      <div className={css(styles.panel)}>
        React + Aphrodite rocks!
      </div>
    );
  }
}
```

### Emotion

[Emotion](https://emotion.sh/) ist die dritte und letzte Bibliothek für
Inline Styling unter React, die hier behandelt wird. Sie bietet dasselbe
wie die Aphrodite, sie nutzt ebenfalls `className` und eine Funktion die
`css` heißt. Anstatt eines Objekt, welches an `className` übergeben
wird, übergibt man ein sogenanntes "tagged Template". Emotion hat den
größten Vorteil, dass es mehr nach CSS aussieht als bei den anderen
beiden Bibliotheken. Es gibt hier bei auch eine weitere Bibliothek die
sich `react-emotion` nennt, diese ermöglicht es einem mit der Funktion
`styled()` eigene HTML zu erstellen und diese unter JSX zu verwenden.

Alles nochmal zusammenfassend als Beispiel:

```jsx
import { css } from 'emotion';

const style = css`
  background-color: #00ffff;
  text-align: center;
  width: 100%;
  padding 20px;
  &:hover {
    color: #ffffff;
    cursor: pointer;
  }
  @media (max-width: 700px) {
    background-color: #ff0000;
  }
`;

class App extends React.Component {
  render() {
    return (
      <div className={style}>
        React + Emotion rocks!
      </div>
    );
  }
```

### CSS Modules

Wir haben aber auch die Möglichkeit die Scopes unserer Styles
anzupassen, hierzu müssen wir, aber paar Konfigurationen anpassen. Dazu
müssen wir `react-scripts` aufrufen.

```bash
react-scripts eject
```

Jetzt passen wir die `webpack.config.js` an in unserem neu generierten
`config`-Ordner. Ab Zeile 391 sollte sie wie folgt aussehen:

```jsx
{
  test: cssRegex,
  exclude: cssModuleRegex,
  use: getStyleLoaders({
    importLoaders: 1,
    sourceMap: isEnvProduction && shouldUseSourceMap,
  }),
  sideEffects: true
}
```

Diese passen wir nun so an, dass der CSS-Loader einzigartige Klassen
daraus generiert.

```jsx
{
  test: cssRegex,
  exclude: cssModuleRegex,
  use: getStyleLoaders({
    importLoaders: 1,
    modules: true,
    localIdentName: '[name]__[local]__[hash:base64:5]',
    sourceMap: isEnvProduction && shouldUseSourceMap,
  }),
  sideEffects: true,
}
```

Alternativ muss man kein `react-scripts` dafür ausführen, mehr dafür
siehe
[hier](https://facebook.github.io/create-react-app/docs/adding-a-css-modules-stylesheet).

Nun müssen wir unseren JavaScript-Code nur noch anpassen. Wir
importieren unsere `App.css`

```jsx
import classes from './App.css';
```

```jsx
return (
    <div className={classes.App}>
    </div>
);
```
