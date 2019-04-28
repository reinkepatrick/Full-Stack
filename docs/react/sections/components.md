## Components

Komponenten sind das grundlegendste in React, auch bekannt als
__Components__. Einzelne Components werden am Ende zu einer App
zusammengesetzt. Der Vorteil an Components ist das man sie immer wieder
verwenden und dynamisch anpassen kann. Jedes Component muss zwingend JSX
zurückgeben oder diesen rendern.

![components](./img/components.svg ':size=525')

Der allgemeine Aufbau eines Components (_src/App.js_) in React sieht wie
folgt aus:

```jsx
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

### Funktionale Components

Diese bezeichnet man als Präsentations- oder zustandslose Components, da
sie in den meisten Fällen nur JSX zurückgeben. Diese Art sollte so oft
wie nur möglich verwendet werden und gilt als Best-Practice, diese
bieten durch React Hooks fast die selben Möglichkeiten wie
klassenbasierte Components.

```jsx
import React from 'react';

const foobar = () => {
  return <p>Foobar!</p>
};

export default foobar;
```

### Klassenbasierte Components

Diese bezeichnet man als Container- oder Zustandscomponents, diese
verwendet man wie der Name schon sagt, wenn man Zustände in einem
Component speichern möchte. Um eigene Components dann zu verwenden, kann
man diese einfach einbinden und aufrufen. Hier zu wird das
funktionale Component aus dem oberen Beispiel genutzt und es in
die `App.js` importiert.

```jsx
import React, { Component } from 'react';
import './App.css';
import Foobar from './Foobar';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>Hello World</h1>
        <Foobar />
      </div>
    );
  }
}

export default App;
```
