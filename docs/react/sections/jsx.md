## JSX

JSX ist die Sprache die zum darstellen von Components verwendet wird, um
genauer zu sagen den Teil den wir in die `render()`-Methode schreiben,
man kann natürlich auch einfach so JSX-Code zurückgeben ohne
`render()`-Methode.

```jsx
render() {
 return (
   <div className="App">
     <h1>Hello World</h1>
   </div>
 );
}
```

Damit wir dort JSX verwenden können importieren wir auch `React` in
unserem Component. React erstellt dann aus dem JSX für den Browser
lesbares JavaScript. Als Beispiel zeige zeige ich euch wie der obere
Code als normales JavaScript aussieht und was React intern mit JSX
macht.

```jsx
render() {
 return React.createElement('div', 
                            { className: 'App' }, 
                            React.createElement('h1', null, 'Hello World'));
}
```

JSX hat aber ein paar kleine Einschränkungen, es sieht zwar aus wie HTML
und es verhält sich auch in den meisten Fällen so, weswegen wir auch zum
Beispiel `className` nutzen anstatt die normale HTML-`class`, weil JSX
intern immer noch zu JavaScript kompiliert wird und `class` unter
JavaScript eine andere Verwendung findet. Außerdem kann man unter JSX im
Normalfall nur ein HTML Element zurückgeben, am folgenden Beispiel sieht
man wie es __nicht__ funktioniert.

```jsx
render() {
 return (
   <div className="App">
     <h1>Hello World</h1>
   </div>
   <p>I'm not working</p>
 );
}
```

Aus diesem Grund packt man um ein Component immer ein HTML-Element und
fügt sein Kontent in dieses HTML-Element oder man nutzt `React.Fragment`
anstatt es in ein `<div></div>` zupacken.
