## Properties

Eigenschaften, auch __Properties__ oder Props genannt, bieten uns die
Möglichkeit unseren Inhalt der Components dynamisch anpassbar zu machen.
Diese kann man einfach bei den Aufruf des Components übergeben.

```jsx
<Foobar foo="bar" />
<Foobar foo="bar" >Foobar</Foobar>
```

![components](./img/props.svg)

Diese Properties kann man dann einfach innerhalb des Components
verwenden. Im zweiten Aufruf übergeben wir etwas nicht als Attribut,
dies kann man einfach über `.children` aufrufen.  
__Wichtig:__ Sollte man dasselbe bei einem klassenbasierten Component
machen, dann muss man in dem unten gezeigten Beispiel `this.props.foo`
verwenden.

```jsx
import React from 'react';

const foobar = (props) => {
  return (
    <div>
      <p>Foo{props.foo}!</p>
      <p>{props.children}</p>
    </div>
  );
};

export default foobar;
```

### State-Property

Die State-Property ist eine besondere Property in React, sie
funktioniert wie ein Objekt in JavaScript, du kannst Sachen drin
speichern und Abrufen. Das besondere daran ist das wenn du den Inhalt
ändert löst das ein rendern im UI aus.

```jsx
class App extends Component {
  state = {
    foo: [
        'bar'
    ]
  }
    
  render() {
    return (
      <div className="App">
        <h1>Hello World</h1>
        <Foobar foo={this.state.foo[0]} />
      </div>
    );
  }
}
```

Um dafür zu sorgen das React das Ändern des State-Property mitbekommt
biete uns React, durch die Vererbung von `Component`, die Methode
`this.setState`. Diese Methode fügt dann das alte State-Property und das
neue State-Property zusammen, es werden nur die Objekte verändert die
auch wirklich durch `this.setState` verändert wurden.

Um States unter funktionalen Components verwenden zu können benutzt man
`useState`, hier ist aber wichtig das das neue State-Property das alte
__überschreibt__. `useState` kann dafür beliebig oft verwendet werden.

### PropTypes

ProTypes ist eine Bibliothek, die es ermöglicht festzulegen welche Typen
die zu übergebenden Properties haben sollen und warnt den Entwickler,
wenn diese nicht eingehalten werden. Diese Bibliothek ist von der React Community
entwickelt worden und muss nachinstalliert (`prop-types`) werden.

```js
Greeting.propTypes = {
  name: PropTypes.string
};
```

Für weitere Typen die PropTypes mitliefert, siehe [hier](https://reactjs.org/docs/typechecking-with-proptypes.html).