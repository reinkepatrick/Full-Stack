## Lists

Dieser Teil wird erklären wir man Werte von Forms verändern kann, wie
If-Bedingungen und For-Schleifen unter React verwendet werden können.

### Two-Way Databinding

Im Vergleich zu Angular besitzen wir hier kein ngModel welches uns die
Arbeit dafür abnimmt das heißt wir müssen selbst auf Änderungen
reagieren und Werte setzen.

Als erstes benötigen wir einen Event-Handler um auf diese Aktion
reagiert, also wenn etwas in das Input-Feld etwas eingetragen wird, in
dem Beispiel wollen wir einen Namen in unserer State-Property ändern.

```jsx
nameChangedHandler = ( event, id ) => {
  const personIndex = this.state.persons.findIndex(p => {
    return p.id === id;
  });

  const person = {
    ...this.state.persons[personIndex]
  };

  person.name = event.target.value;

  const persons = [...this.state.persons];
  persons[personIndex] = person;

  this.setState( { persons: persons } );
}
```

Diese Methode übergeben wir nun beim Rendern unserer Person Component,
hier ist wichtig das wir eine `Key` Attribut mit übergeben, das hilft
React dabei nur die geänderten Teile neu zu rendern.

```jsx
<Person
  name={ person.name } 
  age={ person.age }
  key={ person.id }
  changed={ (event) => this.nameChangedHandler(event, person.id) } />
```

Das Component kann nun auf das Attribut reagieren.

```jsx
<input type="text" onChange={ props.changed } value={ props.name } />
```

### If-Bedingungen

Auch dies ist etwas ungewohnter als unter Angular, wir haben hier keine
Directives (_ngIf_). Wir nutzen einfaches JavaScript, es bietet uns hier
zwei Möglichkeiten. Die erste Möglichkeit ist es die If-Bedingung direkt
im JSX einzubinden.

```jsx
return (
  <div className="App">
    { 1 + 2 === 4 ? <div>Hello World</div> : null }
  </div>
);
```

Die zweite Möglichkeit ist das ganze auszulagern.

```jsx
if ( 1 + 2 === 4 ) {
  helloWorld = (
    <div>Hello World</div>
  );
}

return (
  <div className="App">
    { helloWorld }
  </div>
);
```

![components](./img/if.svg)

### For-Schleifen

Diese können unter React wirklich simpel durch die JavaScript Methode
`map` verwirklicht werden, diese liefert jedes Array unter JavaScript
mit.

```jsx
if ( this.state.showPersons ) {
  persons = (
    <div>
      {this.state.persons.map((person, index) => {
        return <Person
          name={ person.name } 
          age={ person.age }
          key={ person.id }
          changed={ (event) => this.nameChangedHandler(event, person.id) } />
      })}
    </div>
  );
}
```
