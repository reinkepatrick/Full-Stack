# Dotnet Core

## Einleitung

### Was ist dotnet Core?  

dotnet Core ist ein Opensource Framework. Dieses Framework soll das dotnet Framework ersetzen und grundsätzliche Änderungen durchführen. Eine dieser Funktionen ist es Plattformunabhängig zu sein. Das dotnet Team stellt eine CLI zur Verfügung um ein Projekt zu erstellen, Kompilieren und testen. Das dotnet core Framework kann mit VB, C# und F# genutzt werden. Außerdem wurden die einzelnen Komponenten von dem Grundsystem endkapselt.

### Wer entwickelt dotnet core?

Die Entwicklung wird von Microsoft koordiniert der Programm Manager ist Richard Lander. Es handelt sich bei dotnet core um ein Opensource Produkt somit kann jeder Nutzer an der Entwicklung teilnehmen.

### Weiter Entwicklung

Das dotnet Core Team hat für die Version 3.0 die in der zweiten Hälfte 2019 veröffentlicht werden soll vorgesehen Desktop Gui Entwicklung, Razorkomponenten , c# 8.0 support und noch vieles mehr.

Eine liste aller Änderungen sind [hier](https://docs.microsoft.com/de-de/dotnet/core/whats-new/dotnet-core-3-0) erhältlich.

## getting started

Download und Installation kann [hier](<https://dotnet.microsoft.com/download>) gefunden werden.

nach der Installation söllte die dotnet CLI verfügbar sein.

Um jetzt zu beginnen bietet die CLI die Funktion ein Projekt anhand eines Templates vorzubereiten. Um eine Liste aller Templates zu erhalten gibt es den Befehl: 

``` bash
dotnet new --list
```

![Liste in der Eingabeaufforderung Windows](img/dotnet-template-list.png)

In dieser Tabelle kann erkannt werden für welche Programmiersprache dieses Template zur Verfügung steht.



``` bash
dotnet new webapi -lang F#
```



## Funktionen  (Platzhalter)

### interesante packages
#### System
die Klassen für die primitiven Datentypen,
Einige Exception Klassen,
die Random Klasse und
Tuppel

_Beispiel werden noch hinzugefügt_
#### System.Collections.Generics
Unterschiedlche Generische Datenstrukturen z.b. List, Stack, Map, Queue

_Beispiele werden noch hinzugefügt_
#### System.Linq
Das einbinden dieses Namespaces erlaubt Linq zu nutzen. Es muss nicht für Ling to sql ein anderes Namespace eingebunden werden.
#### (System/Microsoft).EntityFrameworkCore
Alle EntityFramework relevanten Klassen
z.b. DBContext







### Codebeispiele

#### F#

##### Variablen

``` F#
let Name = Wert
let ``besonderer Name`` = Wert

let funktion a = a
```

##### Enum / Typen

``` F#
type enum = 
	| A 
	| B 
	| C

type Typ = { Vorname:string ; Nachname:string; alter:int }
let beispielNutzer:Typ = { Vorname = "Beispiel"; Nachname = ""; alter = 0 }
```

##### Funktionen

```F#
let readInput = 
    printfn ":"
    Console.ReadLine()

let parse a = double a

let calc a = a ** a

let print a = printfn "Die Zahl ist %f" a
```

Parameter und Rückgabewerte benötigen keinen Typen diese Aufgabe übernimmt der Compiler. 

| Funktion  | Parameter | Rückgabewert |
| --------- | --------- | ------------ |
| readInput | -         | string       |
| parse     | string    | double       |
| calc      | double    | double       |
| print     | string    | -            |

##### Pipelining

Pipelining dient dazu lesbare aneinander Reihungen von Funktionen zu schreiben.

``` F#
let reihenfolge  = readInput |> parse |> calc |> print

reihenfolge 
```

##### rekursive Funktionen

In der Grundkonfiguration kann eine Funktion nicht von sich selbst aufgerufen werden. Um diese Funktionalität zu gewähren muss das Keyword **rec** vor dem Funktionsnamen geschrieben werden.

Hier ein Beispiel mit der [McCarthy91](https://en.wikipedia.org/wiki/McCarthy_91_function) Funktion

``` F#
let rec McCarthy91 a = 
    if a > 100 then a - 10 
    else McCarthy91 (McCarthy91 (a + 11))
```

##### Linq

F# unterstützt nur den Querysyntax und nicht den Funktionssyntax. 

``` F#
let list = ["Beispiel"; "Text"; "ab"]
let abfrage = query { 
    for text in list do 
    where (text.Length > 3) 
    select text 
    }
abfrage |> Seq.iter (fun x -> printfn "%s" x)
```

Für eine Sammlung aller Keywords und 

#### Entity Framework
```c#
class Context : DbContext
{
   private const string connectionString = @"Data Source=:memory:;";
   protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
   {
      // dotnet core configuration was für ein Server benutzt werden sll
      optionsBuilder.UseSqlite(connectionString);
      // optionsBuilder.UseSqlServer(connectionString)
   }
   public DbSet<Nutzer> Nutzer { get; set; }
   public DbSet<Nachricht> Nachrichten { get; set; }
}
```

```c#
class Nutzer
{
   public int id { get; set; }
   public string name { get; set; }
   private ICollection<Nachrichten> Nachrichten { get; set; }
}

class Nachrichten
{
   public int id { get; set; }
   public string nachricht { get; set; }
   public Nutzer Nutzer { get; set; }
}
```

```c#
class Program
{
   static void Main(string[] args)
   {
      var db = new Context();
      // Datenbank auf den neuesten Stand Updaten
      db.Database.Migrate();
      // Einen Nutzer anlegen und fürs speichern in der Datenbank vormerken
      db.Nutzer.Add(new Nutzer{ id=1, name="Text" });
      // Alle gemerkten Änderungen in der Datenbank abspeichern
      db.SaveChanges();
   }
}
```

#### Linq

##### Funktionssyntax

```c#
var list = new List<string>();
list.Where((x) => x.length > 5)
```

```c#
var list = new List<string>();
list.OrderBy((x) => x.length)
```

```c#
var list = new List<string>();
list.Select((x) => x.length)
```

##### Lambda

Funktionssyntax ist nicht zu verwechseln mit lambda welches in etwa zur selben zeit eingeführt wurde und vom syntax ähnlich aussieht. und schnell zu verwirrungen führen kann

```c#
var list = new List<string>();
list.foreach((x) => Console.WriteLine(x))
```
Besonders Interessant ist auch dieser eigentliche Select der aber Lambda ist und nicht Query 
```c#
var list = new List<string>();
list.FirstOrDefault((x) => x.length)
```
##### Querysyntax

Der größte Funktionale unterschied ist das der Query syntax wie ein Datenbank Query genutzt werden kann. also Sortieren, auswählen, Filtern, Gruppieren.

```c#
var list = new List<string>();
var element = from text in list where text == "Beispiel" select text;
```



## Technologien

#### Entity Framework

Ein von Microsoft entwickeltes Objekt Relations Model. Es enthält alle Grundlegenden Funktionen alle CRUD Operationen, Transaktionen, Migrationen

#### Linq

Linq ist ein Teil der dotnet-Programmiersprachen und ermöglicht einfachen Zugriff auf Daten verschiedener Quellen( z.b. DatenbankTabellen, XML, Listen ). Unter diesen Funktionen gelten filtern, gruppieren, sortieren, selektieren. Es gibt 2 schreibformen einmal Querysyntax welcher sehr ähnlich ist wie Datenbanken query sprachen wie sql und funktionssyntax welcher identisch ist zum lambda syntax.

#### IIS (Internet Information Service)

Ein Service welcher Daten für das Netzwerk bereitstellen kann und mit im Grundumfang bereits einige Hilfreiche Funktionen beinhaltet z.b. HTTP, HTTPS, FTP, Acitve Directory.  

#### Programmiersprachen

##### C# #

C# ist eine Simple und Moderne Objekt Orierntierte Programmiersprache die Typ sicher ist.

##### F# #

F# ist eine Plattform unabhängige funktionale Programmiersprache. Welche außerdem ein Open-Source Projekt ist. F# kann genutzt werden um Objekt Orientiert oder Imperativ zu programmieren.

##### VB (Visual Basic)

Visual Basic ist eine Programmiersprache mit Simplen Syntax um Typ sichere und Objekt Orientierte Anwendungen zu entwickeln.

## Glossar

### CLI

Ein Command Line Interface in kurz CLI ist eine Anwendung die nur in der Konsole des Betriebssystems gesteuert werden kann.  

## Quellen

[dotnet core wikipedia](https://de.wikipedia.org/wiki/dotnet_Core)

[Linq wikipedia](https://de.wikipedia.org/wiki/LINQ)

[IIS Server wikipedia](https://de.wikipedia.org/wiki/Microsoft_Internet_Information_Services)

[Microsoft dotnet erklärung](https://dotnet.microsoft.com/learn/dotnet/what-is-dotnet)

[F# Github Organisation](https://github.com/fsharp)

[C# Querysyntax](https://www.tutorialsteacher.com/linq/linq-query-syntax)