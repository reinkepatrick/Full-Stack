# F# code schnipsel #


### Variablen

Der F# Compiler lägt fest welcher Datentyp benutzt werden muss. Es gibt außerdem kein implizites Casting. Der Name für eine Variable oder Funktion kann jeden Buchstaben enthalten. Außerdem ist es möglich mit 2 Backticks am Anfang auch Sonderzeichen zu verwenden.

``` fsharp
let Name = Wert
let ``besonderer Name`` = Wert

let zahl:int = 0

let funktion a =
	let x = a * 2 in // lokale Variable
	x

let rec fib i =
   match i with
   | 1 -> 1
   | 2 -> 1
   | n -> fib(n-1) + fib(n-2)
```

### Schleifen

In F# gibt es drei arten von schleifen Zähler gesteuerte Schleife, Kopfgesteuerte Schleife und für jedes Element Schleife. Diese Schleifen existieren zwar sind aber in der Funktionalen Programmierung selten bis gar nicht verwendet.

``` fsharp
for i = 1 to 10 do
    printf "%d " i
for i = 10 downto 1 do
	printf "%d " i
```

``` fsharp
while Bedingung do
    Anweisungen
```

Für jedes Element
``` fsharp
for pattern in enumerable-expression do
    body-expression

for (i,j) in [(1,2),(3,4)] do
	printf "first: %i" i
    printf "second: %i" j

let sum = 0
for (_,_) in [(1,2),(3,4)] do
	printf "Parameter werden nicht benötigt"
    sum += 1

```
### Funktional Iterieren
``` fsharp
List.iter (fun x -> printf "%i" x) [1..10]
```

### Enum / Typen

Typen sind ähnlich wie Objekte. Der größte unterschied ist das es weder Konstruktor noch Funktionen für diese Objekte gibt.

``` fsharp
type enum =
	| A
	| B
	| C

type Typ = { Vorname:string ; Nachname:string; alter:int }
let beispielNutzer:Typ = { Vorname = "Beispiel"; Nachname = ""; alter = 0 }
```

### Funktionen

Funktionen in F# haben immer mindestens einen Parameter und immer einen Rückgabe wert. Um eine Funktion ohne Parameter zu erstellen kann der typ unit benutzt werden welcher das äquivalent zu null in anderen Sprachen ist.

```fsharp
let readInput() =
    printfn ":"
    Console.ReadLine()

let parse a = double a

let calc a = a ** a

let print a = printfn "Die Zahl ist %f" a
```

Parameter und Rückgabewerte benötigen keinen Typen diese Aufgabe übernimmt der Compiler.

| Funktion  | Parameter | Rückgabewert |
| --------- | --------- | ------------ |
| readInput | unit      | string       |
| parse     | string    | double       |
| calc      | double    | double       |
| print     | string    | unit         |



#### Pipelining

Pipelining dient dazu lesbare aneinander Reihungen von Funktionen zu schreiben.

``` fsharp
let reihenfolge  = readInput |> parse |> calc |> print

reihenfolge
```

#### rekursive Funktionen

In der Grundkonfiguration kann eine Funktion nicht von sich selbst aufgerufen werden. Um diese Funktionalität zu gewähren muss das Keyword **rec** vor dem Funktionsnamen geschrieben werden.

Hier ein Beispiel mit der [McCarthy91](https://en.wikipedia.org/wiki/McCarthy_91_function) Funktion

``` fsharp
let rec McCarthy91 a =
    if a > 100 then a - 10
    else McCarthy91 (McCarthy91 (a + 11))
```
### Pattern Matching

Pattern matching kann wie ein Switchcase genutzt werden.

```fsharp
match i with
    | 0 -> 0
    | 1 -> 1
    | 2 -> 4
    | n -> n*n
```

Es gibt eine Erweiterung welche Active Pattern heißt und es erlaubt Funktionen oder Bedingungen als Case zu definieren.

```fsharp
let rec McCarthy91 a =
	match a with
    | n when a > 100 -> n-10
    | n -> McCarthy91 (McCarthy91 (n + 11))
```



### F# Magie Active Pattern

Hier ein Umgekehrte polnische Notation Rechner welcher einen String erhält und alle Grundrechenarten auf diesen anwenden kann. [Hier](<https://de.wikipedia.org/wiki/Umgekehrte_polnische_Notation>) gibt es eine Erklärung der Notation.

``` fsharp
let RPNcalculator(s : string) =
    let solve items current =
        match (current, items) with
        | "+", y::x::t -> (x + y)::t
        | "-", y::x::t -> (x - y)::t
        | "*", y::x::t -> (x * y)::t
        | "/", y::x::t -> (x / y)::t
        | _ -> (float current)::items
    (s.Split(' ') |> Seq.fold solve []).Head
```
