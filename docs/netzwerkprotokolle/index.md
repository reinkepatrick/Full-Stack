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
        	"Full STack Development",
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

| Methode | Beschreibung     |
| ------- | ---- |
|   GET      | Daten von Server anfordern |
| POST| Daten an Server übermitteln |
| PUT| Daten auf den Server ändern |
|DELETE | Daten auf den Server löschen |

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

REST hat auch mit einigen Limitierungen zu kämpfen. So kommt es unter REST oft dazu, das man mehrere zusammenhängende Abfragen nacheinander stellen muss und sie nicht gleichzeitig stellen kann.  Möchte man beispielsweise Informationen über ein Produkt und gleichzeitig über den Hersteller abrufen, muss man zwei Anfragen stellen.  <br/>Außerdem ist es unter REST nicht möglich nur bestimmte Informationen über eine  Ressource abzufragen. Hat ein Hersteller also einen Namen, Adresse und ein Gründungsjahr, wir wollen aber nur den Namen wissen, bekommen wir trotzdem den ganzen Datensatz geliefert, auch Overfetching genannt.  <br/>Zusammengefasst könnte man sagen, REST ist sehr unflexibel. 

## GraphQL	

## GRPC
## HTTP 3
## Websockets

## Quellen
- [GraphQL Guide mit interaktiven Beispielen](https://graphql.github.io/learn/)

- [Grundlagen Netzwerkprotokolle](<https://de.wikipedia.org/wiki/Netzwerkprotokoll>)

- [Grundlagen JSON](<https://www.w3schools.com/js/js_json_intro.asp>)

- [Grundlagen REST](<http://www.codeadventurer.de/?p=3228>)

  
