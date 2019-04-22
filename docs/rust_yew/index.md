# Rust und Yew

## Was ist Rust?
[Rust](https://www.rust-lang.org/) ist eine Multiparadigmen-Programmiersprache (generisch, nebenläufig, funktional, imperativ, strukturiert), mit dem Ziel der Fehlervermeidung zur Kompilierzeit mithilfe von z.B. kontrollierten Speicherzugriffen.

## Was ist Yew?
[Yew](https://github.com/DenisKolodin/yew) ist ein in Rust geschriebens Framework für die Erstellung von Frontend Apps mit WebAssembly. Das Framework unterstützt multi-threading sowie Nebenläufigkeit und benutzt die Web Workers API zur Erstellung von agents in seperaten Threads.

<details>
    <summary> <h2>Grundlagen Rust</h2> </summary>
Grundlagen und sprachspezifische Eigenschaften von Rust (basierend auf https://doc.rust-lang.org/book)

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
Wenn die Funktion kein Rückgabewert hat wird `-> Rückgabetyp` weggelassen. Zudem wird in Rust kein `return` o.Ä. gebraucht, da der letzte wert im Ausdruck als Rückgabewert gesehen wird:
```
fn five() -> i32 {
  5
}
```
Rückgabewerte in Ausdrücken werden nicht von einem `;` beendet, somit wäre folgendes falsch:
```
fn five() -> i32 {
  5;
}
```
Fehlermeldung:
```
error[E0308]: mismatched types
 --> src/main.rs:7:28
  |
7 |   fn plus_one(x: i32) -> i32 {
  |  ____________________________^
8 | |     x + 1;
  | |          - help: consider removing this semicolon
9 | | }
  | |_^ expected i32, found ()
  |
  = note: expected type `i32`
             found type `()`
```
Fehlt der Ausdruck wird ein leerer Tupel zurückgegeben und wird auch `statement` anstatt `expression` genannt

Ausdrücke können auch mitten in einer Funktion vorkommen z.B.:
```
fn something() {
  let i = { // anfang Ausdruck
    5
  }; // ende Ausdruck
  println!("value: {}", i); // value: 5
}
```

##### Macros sind keine Funktionen
Macros wie z.B. `println!()` werden mit einem `!` gekennzeichnet und sind nicht Funktionen. Genaueres zu Macros wird später geschrieben.

#### Kommentare
Kommentare werden nur mit `//` gekennzeichnet und können überall im Code stehen. Alles folgende nach `//` wird als Kommentar gesehen und nicht als Programmcode.

#### Kontrollfluss

##### If-Ausdrücke
If-Ausdrücke werten einen bool aus und führt dann einen der Zweige (auch `arms` genannt) aus.
```
let num = 3;
if num < 5 {
  println!("condition true");
} else {
  println!("condition false");
}

Ausgabe: condition true
```
Ist der auszuwertende Wert kein bool wird ein Fehler zur Kompilierzeit erkannt, da Rust nicht versucht den Wert in ein bool zu casten sondern in erster Linie nur bools akzeptiert.
Es können mehrere Konditionen mit `else if` kombiniert werden, es wird aber nur der erstzutreffende Arm ausgeführt:
```
let number = 6;
if number % 4 == 0 {
    println!("number is divisible by 4");
} else if number % 3 == 0 {
    println!("number is divisible by 3");
} else if number % 2 == 0 {
    println!("number is divisible by 2");
} else {
    println!("number is not divisible by 4, 3, or 2");
}

Ausgabe: number is divisible by 3
```

Man kann If-Ausdrücke auch mit `let` kombinieren, solange die erwarteten Rückgabewerte den selben Typen besitzen:
```
let condition = true;
let number = if condition {
    5
} else {
    6
};

println!("The value of number is: {}", number);

Ausgabe: The value of number is: 5


let condition = true;
let number = if condition {
    5
} else {
    "six"
};

Fehler: error[E0308]: if and else have incompatible types
note: expected type `{integer}`
             found type `&str`
```

##### Schleifen

###### loop
`loop` ist eine Endlosschleife, aus der nur mit `break` rauskommt.
```
loop {
    counter += 1;

    if counter == 10 {
        break;
    }
};
```

`loop` kann man auch wieder mit `let` verbinden:
```
let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};
```

###### while
`while` wird so lange ausgeführt bis die gegebene Kondition `false` ist.
```
let mut number = 3;

while number != 0 {
    println!("{}!", number);

    number = number - 1;
}
```

###### for
Für den durchlauf z.B. eines Arrays ist nutzung von `while`-Schleifen riskant und es wird eine `for`-Schleife bevorzugt. sie ermöglicht es Entweder durch ein Array o.Ä. oder durch eine `range` von Zahlen zu iterieren.
```
let a = [10, 20, 30, 40, 50];

for element in a.iter() {
    println!("the value is: {}", element);
}

for n in 0..101 { // von 0 bis 100
  //code
}
```
<details>
