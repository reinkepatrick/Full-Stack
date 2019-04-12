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

