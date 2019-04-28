## Styling

Wie unter normalen HTML gibt es auch hier die Möglichkeit Inline Styling
oder via externe `.css`-File Stylings vorzunehmen. Als erstes wird das
Styling via `.css`-File an dargestellt.

Hier für muss nur die `.css`-File importiert werden.

```jsx
import React from 'react';
```

Nun kann man die Klassen via `className` verwenden.  
__Wichtig hier bei ist das alle Änderungen in der `.css`-File global sind.__  
Für das Inline Styling muss eine Anweisung in JavaScript
verfasst werden.

```jsx
const style = {
  backgroundColor: 'white',
  border: '1px solid blue'
};
```

Jetzt muss dies nur noch in den JSX Code verwendet werden.

```jsx
return (
  <div className="App">
    <h1 style={ style }>Hi, I'm a React App</h1>
  </div>
);
```

### Radium

Radium ist eine Reihe an Tools, die die Möglichkeit bieten
Inline Styles in einem React Component zu verbessern. Diese bauen
außerdem auch Pseudo Selektoren, Media Queries und vieles mehr als
Inline Styling ein.

Um es aber nun auch verwenden zu können muss es in der Datei, wo
es verwendet werden soll, noch eingebunden und das Component beim
zurückgeben darin eingepackt (_wrapping_) werden.

```jsx
import Radium from 'radium';
```

```jsx
export default Radium(App);
```

Nun können Pseudo Selektoren in Inline Styling verwenden.
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

Als nächstes wird dargestellt wie man Media Queries unter Radium und
React verwendet, als Inline Styling.

```jsx
const style = {
  '@media (min-width: 500px)': {
      width: '450px'
  }
};
```

Um Media Queries verwenden zu können, muss die App in ein `StyleRoot` gepackt werden,
dasselbe gilt auch für Keyframes.

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

Es gibt auch die Möglichkeit die Scopes der Styles
anzupassen, hierzu müssen, aber paar Konfigurationen angepasst werden. Dazu
muss `react-scripts` aufgerufen werden.

```bash
react-scripts eject
```

Jetzt wird noch die `webpack.config.js` angepasst, diese ist im neu generierten
`config`-Ordner. Ab _Zeile 391_, sie sollte wie folgt aussehen:

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

Diese wird so angepasst, dass der CSS-Loader einzigartige Klassen
daraus generieren kann.

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

Nun muss nur noch der JavaScript-Code angepasst werden,
dies wird unten anhand eines Beispiels gezeigt. 

```jsx
import classes from './App.css';
```

```jsx
return (
    <div className={classes.App}>
    </div>
);
```

