# Dotnet Core

## Einleitung

### Was ist dotnet core

dotnet Core ist ein Open source framework welches die Technologien des Microsoft Produktes dotnet framework als einzelne Komponenten anbietet. Der Ansatz von dotnet core ist es eine ähnlichen Ansatz wie Java zufahren nur ohne Virtuelle Maschine. Der Quellcode muss nur einmal geschrieben werden und kann dann native für das jeweilige Betriebssystem kompiliert werden. Eine der ersten Funktionen die Implementiert wurden war die Webserver Möglichkeit sowie das Entity Framework. Wichtig zu wissen ist auch das dotnet core nicht nur von einer Programmiersprache verwendet werden kann sondern von mehreren. Microsoft bietet aktuell C#, F# und VB als mögliche Programmiersprachen an.

### Wer entwickelt dotnet core

Das Projekt wurde von Microsoft gestartet und wird vom Programm Manager Richard Lander betreut. Aufgrund das dotnet core ein öffentlich zugängliches Projekt ist und auf github bereit gestellt wurde, gibt es bereits einige Entwickler außerhalb von Microsofts dotnet core teams die zur Verbesserung und weiter Entwicklung des Frameworks beigetragen haben. 

### Crossplattform

Aufgrund der großen Überarbeitung des dotnet Framework und kompletter Auftrennung aller Funktionalitäten  in eigene Pakete kann jede Plattform beim nativen kompilieren die für die Plattform notwendigen Pakete austauschen. 

### Weiter Entwicklung

Das dotnet Core Team hat für die Version 3.0 die in der zweiten Hälfte 2019 veröffentlicht werden soll vorgesehen Desktop Gui entwicklung, Razorkomponenten , c# 8.0 support und noch vieles mehr.

Eine liste aller Änderungen sind [hier](https://docs.microsoft.com/de-de/dotnet/core/whats-new/dotnet-core-3-0) erhältlich.

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

## Quellen

[dotnet core wikipedia](https://de.wikipedia.org/wiki/dotnet_Core)

[Linq wikipedia](https://de.wikipedia.org/wiki/LINQ)

[IIS Server wikipedia](https://de.wikipedia.org/wiki/Microsoft_Internet_Information_Services)

[Microsoft dotnet erklärung](https://dotnet.microsoft.com/learn/dotnet/what-is-dotnet)

[F# Github Organisation](https://github.com/fsharp)

[C# Querysyntax](https://www.tutorialsteacher.com/linq/linq-query-syntax)