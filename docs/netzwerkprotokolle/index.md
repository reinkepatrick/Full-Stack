# Kommunikation in Verteilten Systemen

von André Matutat 

## Vorwort
Ein Clientbasiertes User Interface mit Anbindung an einen Server, welcher die Business Logik ausführt und einen Datenbankserver im Hintergrund.</br> 
Solche Strukturen sind Alltag in der heutigen Entwicklungszeit, deswegen sollte sich jeder einmal mit den Grundlagen der Kommunikation in verteilten Systemen auseinandersetzten, es schadet aber auch nicht einen Blick abseits des gängigen Standards zu werfen, da es viele junge Technologien gibt. </br>

In dieser Dokumentation möchte ich Grundlagen der Kommunikation auf Anwendungsebene erläutern. Ich werde kurz auf gängige Praxis eingehen, den Fokus aber auf GraphQL legen. </br>

*Anmerkung: Aspekte der Sicherheit werden hier nicht behandelt*

## Grundlagen

Ziel jeder Kommunikation ist  die Übermittlung von Daten, dafür brauchen wir zwei grundlegende Komponente, das Protokoll und das Dateiformat.

### Protokolle

Das Protokoll setzt die Regeln der Kommunikation fest. Es gibt an in welcher Syntax Anfragen und Daten übertragen werden. 

### Dateiformate

Das Dateiformat gibt die Syntax an, wie Daten gespeichert werden müssen. Daraus folgt auch, wie mit den Daten gearbeitet werden kann. 

#### JSON

JSON steht für JavaScript Object Notation und ist ein gängiges Dateiformat, wenn es um die Übertragung von Daten zwischen Server und Webanwendung geht. JSON Files haben den Vorteil, dass sie simpler gehalten und dadurch einfacher zu lesen sind als beispielsweise XML.

Ein typisches JSON File könnte in etwa so aussehen  :

 ```json
{
    "student": {
  		"id": 012,
  		"name": "Beispiel Student"
        "gebDat": "1990-01-01",
        "Wahlfächer": [
        	"KI",
        	"Full Stack Development",
        	"Kunst"
        ]
    }
}
 ```

Im Grunde werden Daten in JSON wie Objekte formatiert. In diesem Beispiel haben wir einen Studenten, der eine id, einen Namen, ein Geburtsdatum und ein Array mit Wahlfächern hat. 

Auch wenn JSON JavaScript Syntax nutzt, ist es rein Textbasiert und dadurch Sprachen unabhängig. 



## REST

REST steht für Representational State Transfer und ist ein aktueller Standard zur Datenübertragung in Webbasierten Anwendungen.<br/>Grundkonzept von REST: Alles ist eine Ressource <br/>Die Funktionalität von REST kann man sich ähnlich vorstellen wie die Funktionalität von Objekten in Java. Der Server implementiert eine RESTful API, sie stellt Methoden zum Datenaustausch bereit. Der Client ruft diese Methoden auf und übergibt ggf. Parameter. <br/>Machen wir ein Beispiel: 

```http
http:\\www.fh-bielefeld.de\stundenplan?fach=informatik
```

Diese Anfrage besteht aus drei Teilen

- der Domäne, quasi das Java Objekt 
- der Ressource, quasi Methode "Stundenplan"
- der Parameter "fach" mit den Wert "Informatik "



REST verwendet Methoden des HTTP um den Zugriff auf Daten zu ermöglichen, die wichtigsten Methoden sind: 

| Methode | Beschreibung                 |
| ------- | ---------------------------- |
| GET     | Daten von Server anfordern   |
| POST    | Daten an Server übermitteln  |
| PUT     | Daten auf den Server ändern  |
| DELETE  | Daten auf den Server löschen |

Welche Methode verwendet wird, legen wir im HTTP Header fest:

```http
POST /shop/products/ HTTP/1.1
Host: www.meinShop.de
Content-Type: application/json

{
  "name": "Kaffee",
  "price": 2.99
}
```

In diesem Beispiel senden wir einen POST Request, also übermitteln wir Daten an den Server. "/shop/products/" gibt die Ressource an auf die wir zugreifen. Die Daten übermitteln wir hier mittels JSON.  <br/>Damit eine REST API eine richtige REST API ist, müssen noch einige Rahmenbedingungen erfüllt werden, die HATEOAS. In der Praxis werden diese Bedingungen nur mangelhaft oder gar nicht erfüllt. Daher spricht man manchmal auch von REST-ish, REST-like oder REST-wannabe.  

### Nachteile von REST

REST hat auch mit einigen Limitierungen zu kämpfen. So kommt es unter REST oft dazu, das man mehrere zusammenhängende Abfragen nacheinander stellen muss und sie nicht gleichzeitig stellen kann.  Möchte man beispielsweise Informationen über ein Produkt und gleichzeitig über den Hersteller abrufen, muss man zwei Anfragen stellen. Bei größeren Datenmenge, kann dies ganz schön langsam sein.<br/>Außerdem ist es unter REST nicht möglich nur bestimmte Informationen über eine  Ressource abzufragen. Hat ein Hersteller also einen Namen, Adresse und ein Gründungsjahr, wir wollen aber nur den Namen wissen, bekommen wir trotzdem den ganzen Datensatz geliefert, auch Overfetching genannt.  <br/>

## GraphQL	

GraphQL ist eine, von Facebook entwickelte, opensource Abfragesprache, dessen Fokus auf einfache und flexible Benutzung liegt. <br/>

Die Facebook App war Anfangs sehr träge, dies lag aber nicht an der App selber, sondern an der Kommunikation zwischen Backend und App. Facebook hat dann damit angefangen eine Komplexe REST Schnittstelle zu entwickeln die unterschiedliche Abfragen zu lies, daraus entwickelte sich GraphQL.<br/>Grundkonzept von GraphQL ist die Vorstellung, dass Daten als Graf dargestellt werden. Wo REST Resourcen mithilfe von Verlinkungen miteinander verbindet, werden diese bei GraphQL über Relationen im Grafen miteinander verbunden. Das erlaubt es GraphQL deutlich flexibler zu sein als REST.<br/>

GraphQL ist unabhängig von der Datenbanksoftware und ist in fast jeder Programmiersprache anwendbar(wie Haskell, JavaScript, Python,Ruby, Java, C#, Scala, Go, Elixir, Erlang, PHP, R und Clojure).

### Abfragen

GraphQL Abfragen verwenden immer die HTTP Methode POST und anders als REST, stellt GraphQL Anfragen immer an dieselbe URL.

#### Standart Abfragen

GraphQL bietet den Client besondere Freiheiten, wenn es darum geht seine Abfragen zu gestalten. <br/>Eine einfache Abfrage in GraphQL könnte in etwa so aussehen: 

``` 
query student{
  student {
    id
    name   
  }
}
```
Und so könnte die Antwort JSON aussehen:
```JSON
{
  "data": {
    "student": {
    	"id": 1,
    	"name": "Andre"      
    }
  }
}
```



Ändern wir die Anfrage etwas ab, könnte sie so aussehen: 

```
query student{
  student {
    name 
    id
    semester
    gebDat
  }
}
```

Die Antwort:

```JSON
{
  "data": {
    "student": {
    	"name": "Andre",  
    	"id": 1,
    	"gebDat": "1990-01-01",   
    }
  }
}
```

Wir haben also die Möglichkeit, nur die gewünschten Daten abzufragen und diese in der von uns gewählten Reihenfolge zurück zu bekommen. Dadurch wird Over- und Underfetching vermieden und der Client weiß ganz genau wie die Antwort aussehen wird. 

#### Verschachtelte Abfragen

GraphQLs zweiter großer Vorteil gegenüber REST wird deutlich, wenn wir uns ein Beispiel anschauen, welches Abfragen über mehrere Datenknoten ausführt.<br/>Wie bereits im JSON Beispiel gezeigt, hat ein Student Wahlfächer, Wahlfächer haben einen Namen, Creditpoints und einen Dozenten, ein Dozent hat wiederum einen Namen, einen Titel und ein Fachgebiet. <br/>Würden wir eine RESTful API verwenden, müssten wir mehrere Abfragen stellen, um all diese Daten abfragen zu können. Durch die Verwendung von GraphQL brauchen wir nur eine: 

```
query student{
  student{
    id
    name
    wahlfach{
      name
      credits
      dozent {
        name
        titel
        fachgebiet
      }
    }
  }
}
```

Und so würde die Antwort aussehen: 

```JSON
{
  "data": {
    "student": {
    	"id": 1,
    	"name": "Andre",    
        "wahlfach": [
          {
            "name": "KI",
            "credits": 5
            "dozent":{
              "name": "Gips",
              "titel": "Prof. Dr",
              "fachgebiet": "Informatik"
         	 }
          }
           {
            "name": "Full Stack Development",
            "credits": 15
            "dozent":{
              "name": "Brunsmann",
              "titel": "Prof. Dr",
              "fachgebiet": "Informatik"
            }
          }
        ]
    }
  }
}
```

Dadurch, dass wir nur eine Abfrage stellen brauchen, nur die Daten bekommen, welche wir wirklich brauchen sparren wir nicht nur Bandbreite sondern können durch die Gewissheit, das Daten in der von uns gewünschten Formatierung ankommen, unsere Applikation optimieren und so Performance auf der Client Seite sparen. <br/>

#### Argumente

Natürlich ist es auch möglich, Abfragen mithilfe von Argumenten zu verfeinern. Haben wir eine Liste aller Wahlfächer, brauchen aber nur die Informationen über das Wahlfach KI, dann würden unsere Anfrage so aussehen: 

```
query wahlfach{
  wahlfach (name:"KI"){    
    credits
  }
}
```

Antwort:

```JSON
{
  "data": {
    "wahlfach": {
    	"credits": 5
    }
  }
}
```

##### Variabeln

Im obigen Beispiel haben wir unsere Argumente statisch in die Abfrage geschrieben. In der Praxis kommt es aber häufig dazu, dass wir Abfragen dynamisch gestalten müssen, beispielsweise wenn bestimmte Einstellungen in der UI getätigt werden. <br/>Theoretisch könnte man seine Abfragen natürlich dynamisch zusammenbauen, dies würde aber gegen das Konzept der einfachen Implementierung sprechen. Deswegen ist es in GraphQL möglich, Argumente mithilfe von Variablen zu füllen.

```
query wahlfach($WFname: String{
  wahlfach (name: $WFname){    
    credits
  }
}
```

Variable Definition: $WFname repräsentiert die Variable, nach den Doppelpunkt folgt der Datentyp. <br/>Natürlich müssen wir unsere Variable noch einen Wert zuweisen, dies machen wir mithilfe einer JSON

```JSON
{
  "WFname": "KI"
}
```

An den Server senden wir sowohl die Query als auch die JSON. <br/>Wir können bei Bedarf auch Standard Variablen festlegen.

```
query wahlfach($WFname: String ="KI"{
  wahlfach (name: $WFname){    
    credits
  }
}
```

#### Richtlinien (Directives)

Variablen ermöglichen es Querys dynamisch anzupassen. Richtlinien ermöglichen es uns, die Struktur von Querys dynamisch anzupassen.<br/>

```
query wahlfach($WFname: String, $dozent: Boolean!{
  wahlfach (name: $WFname){    
    credits
   	dozent @include (if: %dozent){
      name
      titel
      fachgebiet
   	}
  }
}
```

Im Grunde funktioniert dies genauso wie die Verwendung von Argumenten. Die Informationen über den Dozenten werden nur mitgeliefert, wenn die Variable $dozent in der JSON auf true gesetzt wird. Durch das Ausrufezeichen am Ende des Datentyps, legen wir übrigens fest, dass die Variable nicht NULL sein darf. Als Gegenstück zu @include existiert auch noch @skip<br/>Zur Vollständigkeit, eine mögliche dazugehörige JSON 

```JSON
{
  "WFname": "KI",
  "dozent": true
}
```

#### Mutationen

- tbd

### Schemen

- tbd

### Nachteile

Auch wenn GraphQL auf den ersten Blick wie das bessere REST aussieht, hat es aber auch einige schwächen. <br/>Die große Freiheit der Clients, Abfragen nach Bedarf zu formen, kann zu hohen Performance Problemen auf den Backend führen. Serverseitig muss man also ein besonderes Augenmerk auf die Performance legen. <br/>

GraphQL hat aber auch noch mit einigen Kinderkrankheiten zu kämpfen, so gibt es noch keine native Lösung zur Handhabung von Authentifizierungen und natives HTTP Caching ist nicht möglich. Da das Schema nach außen Bekannt gegeben wird, gibt man auch so die Datenstruktur der Applikation preis. <br/>

### GraphQL vs REST

- Wann was verwenden?

## Websockets
## GRPC
## HTTP 3


## Quellen
- [GraphQL Guide mit interaktiven Beispielen](https://graphql.github.io/learn/)
- [Grundlagen Netzwerkprotokolle](https://de.wikipedia.org/wiki/Netzwerkprotokoll)
- [Grundlagen JSON](https://www.w3schools.com/js/js_json_intro.asp)
- [Grundlagen REST](http://www.codeadventurer.de/?p=3228)
- [GraphQL Wikipedia](https://de.wikipedia.org/wiki/GraphQL)

