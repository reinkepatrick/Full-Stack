# Dotnet Core

![ersatz text](./img/dotnetcore-overview.svg)

## Was ist dotnet Core?  

dotnet Core ist eine Opensource Plattform. Die Leitung des Projektes wird von Richard Lander (Programm Manager bei Microsoft) übernommen. Diese Plattform soll in Zukunft das dotnet Framework ersetzen. Eine große Änderung im vergleich zu dem dotnet Framework ist die Plattformunabhängigkeit. Um mit dotnet core arbeiten zu können stellt das dotnet Team eine CLI zur Verfügung. Welche das erstellen von Projekten, kompilieren und testen ermöglicht. Die dotnet core Plattform kann mit VB, C# und F# genutzt werden. Außerdem wurden die einzelnen Komponenten von dem Grundsystem endkapselt.

**Projektstruktur**

Dotnet core nutzt die selbe organisationseinheiten wie das dotnet Framework. Es gibt Solutions welche Projekte enthält und es gibt Projekte welche referenzen(Verknüpfungen von Projekten und Libarys). Die Quellcode Dateien müssen nicht in den Projektdateien eingetragen werden.

**Paketmanager**

Als Paketmanager wird nuget verwendet welches mithilfe von der dotnet CLI einfach zu bedienen ist. Um sich über Packages besser informieren zu können gibt es die adresse: <https://www.nuget.org/packages/>


## Programmiersprache

## F# #

F# ist eine Plattform unabhängige funktionale Programmiersprache. Welche außerdem ein Open-Source Projekt ist. F# kann genutzt werden um Objekt Orientiert oder Imperativ zu programmieren.

F# und funktionale Programmiersprachen versuchen die Arbeit des Programmierers zu vereinfachen indem dieser weniger schreiben muss sowie sich weniger sorgen um Datentypen zu machen.

Beim programmieren mit F# kann man dennoch den Typen angeben um selbst Einschränkungen vorzunehmen sonst erfüllt diese Aufgabe der Compiler. Für eine Liste aller Datentypen stellt Microsoft [Hier](https://docs.microsoft.com/de-de/dotnet/fsharp/language-reference/fsharp-types) eine Liste zur Verfügung.

| Vorteile | Nachteile |
| --- | --- |
| Einfacher zu testen | Kein vollständiges OOP möglich |
| Leicht zu schreibener Code | Pattern matching kann komplex werden |
| Zeit kann sinnvoller eingesetzt werden | |
| Pattern matching | |

## C# #

C# ist eine Simple und Moderne Objekt Orierntierte Programmiersprache die Typsicher ist. Beim Programmieren mit C# kann man viele nützliche Konstrukte Nutzen z.b. delegates, Lambdas, Dynamische Objekte. C# selbst ist ebenfalls Plattformunabhängig. Die meisten Programmierer kennen C# nur im zusammenhang mit dem .Net Framework. Die Programmiersprache c# kann für verschiedenste Anwendungen genutzt werden. Ein paar dieser Möglichkeiten wären:
- Web (.net Framework, .net core)
- Mobile (Xamarin)
- Desktop (.net Framework, .net core)

## VB (Visual Basic) #

Visual Basic ist eine Programmiersprache mit Simplen Syntax um Typ sichere und Objekt Orientierte Anwendungen zu entwickeln. Die bekanntheit von VB ist aus der Microsoft Office Welt wo eine leichte abwandlung von VB verwendet wird mit den namen Visual Basic for Application. 

## Web

### ASP.NET

Ein Framework welches genutzt werden kann um dynamische Webseiten und Webservices zu entwickeln. Es bietet eine eigene Templateengine an mit dem Namen Razor.

ASP.Net nutzt Klassen und Attribute für das Routing.

``` fsharp
[<Route("api/[controller]")>]
[<ApiController>]
type ValuesController () =
    inherit ControllerBase()

    [<HttpGet>]
    member this.Get() =
        let values = [|"value1"; "value2"|]
        ActionResult<string[]>(values)
```

Die Route ist nicht direkt lesbar ohne zu wissen das [controller] durch Values ersetzt wird.
Trotz das Klassen verwendet werden ist keine spezielle Ordner Struktur vorgeschrieben bzw. benötigt welches ein suchen nach der zugehörigen Klasse verursacht. Da die eigentliche Route in der Datei der Klasse deklariert wird.

#### Http Request Pipeline

![https://thomaslevesque.com/2018/03/27/understanding-the-asp-net-core-middleware-pipeline](.\img\Middleware_Pipeline.png)

In ASP.NET ist eine Pipeline verfügbar welche das hinzufügen von Middleware erlaubt. Eine detaillierte Beschreibung zu diesem Thema gibt es [hier](<https://docs.microsoft.com/de-de/aspnet/core/fundamentals/middleware/?view=aspnetcore-2.2>). Ein paar Beispiele für diese Middleware:

- Json Parser
- HTTPS Redirect
- Database Connection

**Unterschiedliche Middlewares**

`Use` erstellt eine uneingeschränkte Middleware und kann von der vorherigen mithilfe von next aufgerufen werden.

`Map` erstellt eine bedingte Middleware Wobei der erste Parameter beim erstellen angibt anhand welchen kriteriums die unterscheidung stattfinden soll.

`Run` erstellt eine Terminale Middleware welche mithilfe von Run aus der vorherigen aufgerufen werden kann.

**Abweichung MVC und Giraffe**

Bei Giraffe werden eigengeschriebene Middlewares mithilfe von Komposition eingebunden. Und werden identisch geschrieben wie zielrouten. Der einzige unterschied ist das kein Return verwendet weden darf.

Hier ein Beispiel wie man einen Datenbank zugriff ermöglichen könnte.

ASP.NET

``` csharp
app.Use(async (context, next) =>
{
    //before Routing
    await next.Invoke();
    //after Routing
});
```


Giraffe:

```fsharp
let connectDatabase =
    fun (next : HttpFunc) (ctx : HttpContext) ->
        task {
            ctx.Items.Add("dbcontext" , dbcontext )
            return None
        }
```

**Middleware vs. Service**

Eine Middleware und ein Service unterscheiden sich gering bis gar nicht. Unter einem Service wird verstanden der Server, Parser, Datenbankverbindung oder Framework. Während eine Middleware z.b. Logging wäre.

Eine Middleware und ein Service können identisch geschrieben werden und es gibt keine Vorgabe ob erst Service oder erst Middlewares vorkommen müssen.

Services bieten meist noch einen Schritt der Konfiguration an und können daher zumindest etwas erkannt werden. Diese Konfiguration kann aber auch in manchen fällen weggelassen werden.


**Server**

Ein **IIS** Server, ist ein Multifunktionsserver welcher sehr viele Zusatz Funktionen mitliefert.

Ein **Kestrel** Server, ist ein Speziell für dotnet core entwickelter Server. Welcher auf reine Performance setzt und Sonderfunktionen außen vorlässt.

Microsoft empfiehlt beim nutzen von Kestrel nach außen einen weiteren Server als Proxy vorzuschalten.

![alt](./img/empfohlene-server-konfiguration.png)

|                            | IIS           | Kestrel                                        |
| -------------------------- | ------------- | ---------------------------------------------- |
| Platform Support           | Windows/Linux | Windows/Linux/Mac                              |
| Statische Webseiten        | Ja            | Ja                                             |
| HTTP Access Logs           | Ja            | Nein                                           |
| Port Sharing               | Ja            | Nein                                           |
| SSL Zertifikat             | Ja            | Intern (Nur zwischen Proxy und Kestrel Server) |
| Windows Authentifikation   | Ja            | Nein                                           |
| Managment Konsole          | Ja            | Nein                                           |
| Prozess Aktivierung        | Ja            | Nein                                           |
| Anwendungsinitialisierung  | Ja            | Nein                                           |
| Request Filtering & Limits | Ja            | Nein                                           |
| IP & Domain Restrictions   | Ja            | Nein                                           |
| HTTP Redirect Rules        | Ja            | Nein                                           |
| WebSocket Protocol         | Ja            | Middleware                                     |
| Response Output Caching    | Ja            | Nein                                           |
| Compression                | Optional      | Optional                                       |
| FTP Server                 | Ja            | Nein                                           |

#### Frontends

möglich sind
 - react
  - mit und ohne redux
 - angular
  - einbinden mithilfe von SinglepageApplication Service
 - razor
  - einbinden mithilfe von SinglepageApplication Service

**Razor**

Eine von Microsoft entwickelte Template Engine. Welche die Programmiersprache der Wahl nutzen kann. Es kann ein Model übergeben werden dieses muss in der ersten Zeile des Dokumentes Definiert sein. Es können weitere Daten übergeben werden mithilfe von Viewbag und  

**Razorkomponente**

Ein Erweiterung von Razor welches erlaubt Komponenten zu entwickeln Ähnlich wie bei Angular. Diese Komponenten können sowohl auf dem Server oder mithilfe von Blazzor als Webassembly beim Klienten Aktionen ausführen.

### Giraffe

![alt](./img/giraffe.png)

Ein auf ASP.NET aufbauendes Framework welches stärker Funktionalen Aspekte einbringt.

Anstelle von dem Routing von ASP.Net kann eine Art Suchbaum erstellt werden.

``` fsharp
let webApp =
    choose [
        subRoute "/api"
            (choose [
                GET >=> choose [
                    route "/Values" >=> handleGetHello
                ]
            ])
        setStatusCode 404 >=> text "Not Found" ]
```

Das Routing kann komplex werden, dennoch kann auf einen Blick erkannt werden, welcher Befehl wohin gehen wird. Routen die Authentifizierung erfordern können auch direkt hier erkannt werden und nicht in einer Funktion welche nur vom Framework intern aufgerufen werden.

## Desktop

Dotnet core wird immer als ein Webframework gesehen. Dies kommt daher das dotnet core als erstes Feature die möglichkeit bekommen hat als Web backend zu dienen. Dotnet core ist keine reine Webbackend Plattform sondern bietet bereits die möglichkeit eine Konsolen anwendung ohne IIS, Kestrel zu erstellen.

Für die Version 3.0 ist angekündigt UWP Anwendungen zu schreiben. Außerdem gibt es Avalonia welches ein wahres Plattformunabhängiges Desktop Framework ist.

## ORM

| | Entity Framework | SqlProvider | Rezoom.net |
| --- | --- | --- | --- |
| Sprachen | C# / VB (F# *) | F# | F# |
| QueryType | SQL / Linq | Linq | pseudo SQL(Typechecked) |
| Datenhandling | Kontext (Sql kann auf Kontext angewand werden) | Kontext | SQL |
| Modelierung | Klassen | Compiletime | Klassen / Compiletime |
| Migration | C#/VB vollwertige Datenbank Migration (erstellt durchs Model) | Keine Datenbank definition | eigen Definierte Sqls |  
| Datenbanktypen | MSSql (Keine Migrationen*nicht dokumentiert*) / SQlite / InMemory / Cosmos / Postgre / Mysql / Firebird / ODBC | MSSql / Sqlite / Mysql / MsAccess / ODBC | Sqlite /  MSSql / Postgre |
| Entwickler | Microsoft |
| Dokumentation | Ausführlich mit vielen guten Beispielen | Ausreichen mit genügend Beispielen | Vielleicht ausreichend schlecht Strukturiert (Beispiele sind entweder 9 Dateien oder zeigen nicht die Relevanten Code schnipsel) |
| Connectionstring kontrolle | Laufzeit | Intellisense / Kompilier | Laufzeit |
| Middleware für ASP.NET | Provider stellt Provider zur verfügung | Nein | Nein |

### Entity Framework (EF)

Ein von Microsoft entwickeltes Framework für Datenbank Operationen für dotnet Framework. Es handelt sich beim Enity Framework um ein Objekt Relations Model. Das EF bietet grundsätzlich zwei Vorgehensweisen an Code-First und Database-First. Es bietet ein CLI Tool welches Migrationen erstellen ermöglicht. Diese Können beim Starten der Anwendung kontrolliert werden. Dies ermöglicht im Laufenden System einfache Änderungen am Datenbank Model.

**Entity Framework Core (EF Core)**

EF Core ist eine Open source Version vom EF. EF Core ist kompatible mit dotnet core.

```csharp
using Microsoft.EntityFrameworkCore;

public class CustomContext : DbContext
{
    public DbSet<User> Blogs { get; set; }
    public DbSet<Message> Posts { get; set; }
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseMySQL("ConnectionString");
    }

    public class User
    {
        public int UserId { get; set; }
        public string Name { get; set; }        
        public List<Message> Messages { get; set; }
    }

    public class Message
    {
        public int MessageId { get; set; }
        public string Content { get; set; }

        public int UserId { get; set; }
        public User User { get; set; }
    }
}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    services.AddDbContext<Models.CustomContext>();
    new TrainingsplanerContext().Database.Migrate();
}

```


### SqlProvider

Keine Migration vom Programm.

```fsharp

type sql = SqlDataProvider<Common.DatabaseProviderTypes.MYSQL,
                connectionString>
let context = sql.GetDataContext()

let UserWithName =
    query {
        for user in context.User do
        where (user.StartWith "A")
        select (user)
    }

let newUser = context.DatabaseName.User.Create()
newUser.Name = "Example"
context.SubmitUpdates()

```

### Rezoom

Migration wird automatisch ausgeführt wenn im Projekt gefunden. Diese müssen selbst geschrieben werden.

``` sql
// v1.model.sql
create table User
    ( UserId int primary key autoincrement
    , Name string(64) null
    );

create table Message
    ( MessageId int primary key autoincrement
    , UserId int references Users(UserId)
    , Content string(512)
    );

create index IX_Message_UserId on Message
    (UserId);

```

``` fsharp

type GetUserSQL = SQL<"""
    select * from User a where a.Name like 'A%'
""">

let getUser() =
    use context = new ConnectionContext()
    let users = GetUserSQL.Command().Execute(context)


```

## Serialisierung

### Newtonsoft JSON.NET

JSON.Net von Newtonsoft ist ein Json Parser Framework welches ein Json to Object Parser beinhaltet sowie Object to Json. Es beinhaltet auch eine Service Komponente welche mit ASP.NET genutzt werden kann.

## Tests

Ein sauber unter dotnet core strukturiertes Projekt ist es für jede Art von Tests (Unit, Integration, End to End) ein eigenes Projekt anzulegen und in diesen das Hauptprojekt zu verlinken.

![Beispiel eines Testsystems](./img/test-pyramid.jpg)

Die Wertigkeit der einzelnen Tests können sehr stark abweichen. Wird z.b. eine Blog software geschrieben dann sind wahrscheinlich weniger Unit Tests notwendig sondern mehr Integrationstests. Gibt es eine Sehr komplexe UI würde ein hoher Wert auf UI tests gelegt.

### Test Frameworks

|                    | NUnit                          | xUnit               | Expecto      |
| ------------------ | ------------------------------ | ------------------- | ------------ |
| Struktur           | OOP                            | OOP oder Funktional | Funktional   |
| Entwickler         | Open Source                    | Open Source         | Open Source  |
| Programmiersprache | c#, VB, f#                     | c#, VB, f#          | F#           |
| Asynchrone Tests   | nein                           | ja                  | ja           |
|                    | Assert.That(1, Is.EqualTo(1)); | Assert.Equal        | Expect.equal |

#### xunit

**Fact**

Ein Test der immer gültig ist und nicht Parameterisiert ist.

**Theory**

Eine Thorie ist ein Test welcher in bestimmten Fällen erfolgreich ist.
Testfälle Können auf drei Vraianten deklariert werden:
 - InlineData (einmalig vor dem Test definiert)
 - ClassData (Eine Klasse von Testdaten)
 - MemberData (Eine methode welche Testdaten generieren kann)

**setup**

Der Konstruktor wird vor jeden Testausgeführt.
Wenn die Testklasse das Interface IDisposable implementiert wird diese nach jedem Test aausgeführt.

Um für die gesammten Tests einen Context zu erstellen gibt es IClassfixtures welche ermöglichen für eine Klasse einmalig etwas zu erstellen und nach allen Tests dieser Klasse wird dies wieeder aufgeräumt.

**Parralelität**

Der XUnit test runner lässt tests unterschiedlicher Klassen parralel laufen. Um bestimmte Tests dazu zubringen das Tests nacheinander ausgeführt werden gibt es Collections welche den Vorteil haben Tests nacheinander auszuführen sowie einen Context zu teilen.

Um Collections einen Context zu geben wird eine CollectionDefinition benötigt.
<https://xunit.net/docs/shared-context>

### Integration tests

In dieser Phase werden zusammengehörige Komponenten bzw. Schnittstellen getestet. Dafür werden meist einzelne Komponenten Simuliert(gemocked). Für Asp.Net gibt es bereits ein Package welches einen Testserver enthält. Mithilfe dieses Testservers kann ein lightweight Server gestartet werden, dieser Server benötigt keinen Port. Mit einen erstellten Klienten für den Server können Anfragen als Json oder Xml versandt werden. Ein Großer Vorteil von diesem System ist es das die selbe Start konfiguration verwendet werden kann wie für das Produktiv System womit Services und Middlewares nur einmal strukturiert werden.


### Ende zu Ende tests

Bei Ende zu Ende tests wird normalerweise immer der Server komplett hochgefahren und das Testen dauert somit am längsten.
Mithilfe von Selenium kann eine Webanwendung auf verhalten in der UI getestet werden. Elemente können direkt über position angesprochen oder über den Dokumentenbaum(Dom). Eine der wichtigen Informationen ist das Selenium nicht ein eigener Browser ist sondern nur eine möglichkeit ist gängige Browser fernzusteuern. Der große Vorteil von Tests mit Selenium ist das nicht nur das Dokument angezeigt wird sondern das auch Verhalten von Steuerelementen getestet werden kann.


## getting started with dotnet core

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



### Microservices

[Micro Services Beispiel von Microsoft](https://docs.microsoft.com/de-de/dotnet/standard/microservices-architecture/)

Microservices ermöglichen Programme zu schreiben welche genau eine Aufgabe erfüllen und auch Ihre eigene Datenbank dafür haben.

Hier ein Beispiel mit einer gemeinsamen API für Mobile und Web App. Zusätzlich wird hier auch gezeigt wie eine ASP.NET Core Anwendung funktionieren könnte.

![micro-service-1](./img/micro-service-1.png)

Ein großer Vorteil von Micro Services ist, dass diese von verschiedenen Anwendungen aufgerufen werden können.

![micro-service-2](./img/micro-service-2.png)

### Codebeispiele

#### F# #

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



#### Linq

Linq ist ein Teil der dotnet-Programmiersprachen und ermöglicht einfachen Zugriff auf Daten verschiedener Quellen( z.b. DatenbankTabellen, XML, Listen ). Unter diesen Funktionen gelten filtern, gruppieren, sortieren, selektieren. Es gibt 2 schreibformen einmal Querysyntax welcher sehr ähnlich ist wie Datenbanken query sprachen wie sql und funktionssyntax welcher identisch ist zum lambda syntax.


## künftige Funktionen

Es gibt eine grosse Liste an neuen funktionen und verbesserungen für 3.0 einige interessante sind:

- UWP Anwendungen
- c# 8.0
- Razorkomponente
- Singe file application

Eine liste aller Änderungen sind [hier](https://docs.microsoft.com/de-de/dotnet/core/whats-new/dotnet-core-3-0) erhältlich.

## Glossar

**CLI**

Ein Command Line Interface in kurz CLI ist eine Anwendung die nur in der Konsole des Betriebssystems gesteuert werden kann.  

**Middleware**

Ein Middleware Programm kann genutzt werden um Daten von Typ a nach Typ b zu konvertieren. z.b. Json to Object und Object to Json.

## Quellen

[dotnet core wikipedia](https://de.wikipedia.org/wiki/dotnet_Core)

[Linq wikipedia](https://de.wikipedia.org/wiki/LINQ)

[IIS Server wikipedia](https://de.wikipedia.org/wiki/Microsoft_Internet_Information_Services)

[Microsoft dotnet erklärung](https://dotnet.microsoft.com/learn/dotnet/what-is-dotnet)

[F# Github Organisation](https://github.com/fsharp)

[C# Querysyntax](https://www.tutorialsteacher.com/linq/linq-query-syntax)

<https://xunit.net/>

<https://nunit.org/>

<https://stackify.com/kestrel-web-server-asp-net-core-kestrel-vs-iis/>

<http://avaloniaui.net/>

<https://www.symbio.com/solutions/quality-assurance/test-automation/>
