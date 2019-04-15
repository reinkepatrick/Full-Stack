# Rust und Yew

## Was ist Rust?
[Rust](https://www.rust-lang.org/) ist eine Multiparadigmen-Programmiersprache (generisch, nebenläufig, funktional, imperativ, strukturiert), mit dem Ziel der Fehlervermeidung zur Kompilierzeit mithilfe von z.B. kontrollierten Speicherzugriffen.

## Was ist Yew?
[Yew](https://github.com/DenisKolodin/yew) ist ein in Rust geschriebens Framework für die Erstellung von Frontend Apps mit WebAssembly. Das Framework unterstützt multi-threading sowie Nebenläufigkeit und benutzt die Web Workers API zur Erstellung von agents in seperaten Threads.

## Grundlagen Rust

### Installation, Updates, Deinstallation

#### Linux und MacOS
Um Rust auf Linux oder MacOS zu Installieren braucht man nur `curl`. Mit ``` curl https://sh.rustup.rs -sSf | sh``` wird der Download und die Installation gestartet. Rust wird automatisch zu den Umgebungsvariablen hinzugefügt.

#### Windows
Man benötigt für die Installation die [Installationsdatei](https://www.rust-lang.org/tools/install) sowie [Build Tools for Visual Studio 2017](https://www.visualstudio.com/downloads/#build-tools-for-visual-studio-2017). Wenn man beides hat muss man nur der Rust Installation folgen.

#### Updates und Deinstallation
Updates können mit dem Befehl `rustup update` installiert werden. Die Deinstallation kann mit dem Befehl `rustup self uninstall` durchgeführt werden.

### Grundliegende Konzepte der Sprache

#### Coding Convention

#### Datentypen

Grundlegend hat Rust zwei Arten von Datentypen:

Skalar Typen:
* Integer
* Float
* Boolean
* Character

und
Verbindungstypen:
* Tupel
* Array

##### Tupel Nutzung

Ein Tupel kann folgend deklariert werden.
```
let Name: (Typ, Typ, Typ, ...) = (Wert, Wert, Wert, ...)
```
Es gibt zwei Möglichkeiten an die Werte eines Tupels ranzukommen.
1. Dot notation
```
let tup: (i8, i8) = (1, 2);
let eins = tup.0;
let zwei = tup.1;
```
2. destructure
```
let tup: (i8, i8) = (1, 2);
let (x, y) = tup;
```

##### Array Nutzung

Ein Array kann folgend deklariert werden.
```
let arr = [1, 2, 3, 4, 5];
let arr2 [i8; 5] = [1, 2, 3, 4, 5];
let arr3 = [3; 5] // [3, 3, 3, 3, 3]
```

Werte eines Arrays können indiziet werden:
```
let a = [1, 2, 3];
let b = a[1]; // b = 2
```

Liegz der Index außerhab des Arraybereichs wird eine Exception geworfen (In Rust auch panic! genannt).

#### Variablen

##### Deklaration
In Rust werden Variablen mit ```let Name: Typ = Wert``` deklariert und initialisiert. Rust unterstützt typinferenz, somit wird oft `: Typ` nicht bei der deklaration gebraucht. Eine Variable ist immer einem Typen zugeordnet, also kann man nicht ein Integer deklarieren und folgden dem ein String zuweisen.

##### Mutability
Alle Variablen sind zunächst immutable, also nicht änderbar nach der ersten initialisierung. Um dies zu umgehen gibt es das Kennwort `mut`, welches die Variablen mutable machen z.B.:
- Falsch:
```
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```
Fehlermeldung:
```
error[E0384]: cannot assign twice to immutable variable `x`
```
- Richtig:
```
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

##### Konstanten
Konstanten in Rust werden mit dem Kennwort `const` gekennzeichnet anstatt mit `let`. Konstanten sind immer immutable. Zudem können Konstanten in jedem scope (auch global scope) deklariert werden und können nur konstante Ausdrücke annehmen und z.B. nicht den Rückgabewert einer Funktion. Außerdem sind Konstanten in ihrem scope valide solange das Programm läuft.

##### Shadowing

In Rust ist es möglich eine Variable mehrmals zu deklarieren, sodass die alte Variable "überschrieben" wird. Somit ist die folgende Implementierung valide:
```
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x); // The value of x is 12
}
```
Dies ist hilfreich wenn man den Typen einer Variable ändern möchte z.B. braucht man eine Variable aus einem struct, also kann man das struct in eine variable laden und kurz darauf die selbe Variable mit hilfe von `shadowing` überschreiben und den Wert der gebrauchten Variable des structs speichern.

#### Funktionen
Der Funktionskopf in Rust sieht im Vergleich zu anderen Sprachen etwas Anders aus:
```
fn Name(param_name: Typ) -> Rückgabetyp {

}
```
Wenn die Funktion kein Rückgabewert hat wird `-> Rückgabetyp` weggelassen. Zudem wird in Rust kein `return` o.Ä. gebraucht, da der letzte Ausdruck als Rückgabewert gesehen wird:
```
fn five() -> i32 {
  5
}
```
Ausdrücke werden nicht von einem `;` beendet.
