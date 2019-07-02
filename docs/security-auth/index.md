# Sicherheit und Authentifizierung 

## Kryptologie

Kryptologie beschäftigt sich mit dem Entschlüsseln und Verschlüsseln von Informationen und dient somit der Informationssicherheit.  

### Kryptographie

Kryptographie beschäftigt sich mit der Sicherheit der Kommunikation gegen Entschlüsselung und Veränderung der Informationen. Es wird sich mit der Konzeption, Definition und Konstruktion von Informationssystemen befasst. Diese Systeme sollen widerstandsfähig gegen Manipulation von Unbefugten.

#### Ziele der Kryptographie:

* Vertraulichkeit / Zugriffsschutz
  * Informationen dürfen nur von Befugten gelesen werden
* Integrität/ Änderungsschutz
  * Informationen müssen vollständig und unverändert sein
* Authentizität/ Fälschungsschutz
  * Urheber der Informationen muss eindeutig Identifizierbar sein 
* Verbindlichkeit/ Nichtabstreitbarkeit
  * Urheber und Absender der Informationen dürfen Urheberschaft nicht abstreiten können

#### Verfahren der Kryptographie:

Klassische Kryptographie:

* Transposition
  * Buchstaben der Botschaft werden anders angeordnet 
* Substitution 
  * Buchstaben werden durch einen anderen Buchstaben oder durch ein Symbol ausgetauscht. 

Moderne Kryptographie:

Es wird nicht mehr mit Buchstaben gearbeitet, sondern mit den einzelnen Bits der Daten, was die Anzahl der möglichen Transformationen erheblich vergrößert. Außerdem können so auch Daten verschlüsselt werden, die keinen Text repräsentieren.

Klassen der modernen Kryptographie:

* Symmetrische Verfahren
  * Verwenden pro Kommunikationsbeziehung einen geheimen Schlüssel für Ver- und Entschlüsselung 
* Asymmetrische Verfahren
  * Verwenden pro Teilnehmer einen privaten und einen öffentlichen Schlüssel. 

### Kryptoanalyse

Bezeichnet die Analyse von kryptographischen Verfahren um diese zu "brechen" oder zu umgehen oder ihre Sicherheit nachzuweisen. 

### Methoden der Kryptoanalyse:

* Brute-Force
  * Ausprobieren aller möglichen Schlüssel. Reihenfolge wird nach der Wahrscheinlichkeit ausgewählt. Es können mehrere Millionen Schlüssel pro Sekunde ausprobiert werden.
* Wörterbuchangriff
  * Passwortsammlung wird speziell für diesen Zweck angelegt und werden nacheinander ausprobiert. Auch hier wird die Reihenfolge nach der Wahrscheinlichkeit ausgewählt. 
* Man-in-the-middle-Angriff
  * Zwischen den Kommunikationspartnern befindet sich der Angreifer und kann Nachrichten mithören und verändern oder neue Nachrichten einfügen.

## Digitale Signatur / Signieralgorithmen

Ein digitales Signaturverfahren berechnet mithilfe eines geheimen Signaturschlüssels (Private Key) aus einer beliebigen Menge Daten einen Wert (digitale Signatur). Mit diesem Wert kann mithilfe des Verifikationsschlüssels (Public Key) absolut bestätigt werden, dass die gesendete Nachricht auch vom richtigen Absender kommt. Um eine Person zuzuordnen, muss der Public Key der Person zugeordnet sein.

### Grundlagen

Aus den Daten und dem Private Key wird durch eine eindeutige Rechenvorschrift die Signatur berechnet. Verschiedene Daten sollten daher mit dem gleichen Key zu einem unterschiedlichen Ergebnis führen. Eine Signatur muss auch für jeden Schlüssel einen anderen Wert ergeben. 

Der Private Key wird auf den Hash-Wert angewandt und nicht auf die Daten direkt. Die Hashfunktion zum berechnen des Hash-Wertes muss kollisionsresistent sein, da es dann fast unmöglich ist aus zwei unterschiedlichen Datensätzen einen gleichen Hash-Wert zu generieren. 

Sobald ein Schlüssel einer Person zugeordnet wurde, kann die Identität des Signaturerstellers über den öffentlichen Zertifikatsserver des Anbieters ermittelt werden.

### Sicherheit

Es ist so gut wie unmöglich, eine Signatur zu fälschen, zu verfälschen oder eine zweite Nachricht zu erzeugen, die für die Signatur auch gültig ist. Ein private Key soll auch nicht aus den Signaturen und dem dazugehörigen Public Key berechnet werden können.

## HMAC

HMAC = Hash-based Message Authentication Code / Keyed-Hash Message Authentication Code

HMAC sorgt dafür, dass Client und Server jeweils über einen private und einen public Key verfügen. Der private Key darf nur dem jeweiligen Server oder Client bekannt sein. Der public Key darf jedoch weitergegeben werden. Dadurch kann genau bewiesen werden, ob der Client berechtigt ist auf den Server zuzugreifen und stellt auch sicher, dass die Nachrichten nicht verändert wurden.

Der Client stellt eine Anfrage an den Server. Dabei berechnet er einen einzigartigen HMAC. Um diesen Wert zu berechnen benutzt der Client die Nachricht, den geheimen Schlüssel und eine Hashfunktion. Nachdem der Server die Anfrage erhalten hat berechnet auch er einen HMAC. Diese beiden Werte werden danach vom Server verglichen. Sind sie identisch, wird der Client als vertrauenswürdig eingestuft und die Anfrage kann ausgeführt werden.

## Authentifizierung

### Authentifizierungsverfahren

+ **Einweg-Authentifizierung**
  + ![](img\Einweg-Auth.png)
  + Alice generiert eine Nachticht M=(TA, RA, IB, d)
    + TA: Zeitstempel von Alice
    + RA: Zufallszahl
    + IB: Identität von Bob
    + d: beliebige Information
    + Daten können zur Sicherheit noch mit dem Public Key von Bob EB verschlüsselt werden
  + Alice sendet (CA, DA(M)) an Bob
    + CA: Zertifikat von Alice 
    + DA: Private Key von Alice 
  + Bob zertifizeirt CA und erhält EA und überprüft ob der Schlüssel abgelaufen ist. 
    + EA: Public Key von Alice
  + Bob entschlüsselt DA(M) mit EA
  + Bob überprüft ob IB in M korrekt ist und TA in M ob Nachricht aktuell ist. 
  + Bob kann nach Bedarf in einer Datenbank nachsehen ob RA schoneinal gesendet wurde um zu überprüfen ob es sich um eine alte Nachricht handelt die erneut gesendet wurde.
+ **Zweiweg-Authentifizierung**
  + ![](img\Zweiweg-Auth.png)
  + Bob generiert eine Nachricht M' = (TB, RA, IA, d)
    + TB: Zeitstempel von Bob 
    + IA: Identität von Alice
    + RA: Zufallszahl aus Schritt 1
    + Daten können noch einmal mit Alices Public Key EA verschlüsselt werden
  + Bob sendet Nachricht DB(M') mit EB um Integrität und Unterschift von Bob zu verfizieren
  + Alice überprüft ob IA in M' korrekt ist 
  + Alice überprüft TB ob Nachricht aktuell ist und kann bei Bedarf auch Zufallszahl überprüfen
+ **Dreiweg-Authentifizierung**
  + ![](img\Dreiweg-Auth.png)
  + Alice vergleicht empfangene RA mit der gesendeten RA 
  + Alice sendet DA(RB) an Bob
  + Bob dechiffriert DA(RB) mit EA
  + Bob vergleicht empfangene RB mit gesendeter RB 

Identitäten lassen sich im Netz nur schwer beweisen und die meisten Websites oder Dienste verwenden unterschiedliche Techniken der Authentifizierung.

Meistens kann man die Identität einer Person über drei Faktoren bestimmen: 

* Wissensfaktor - Eine Information 
  * Dazu gehört das Passwort und auch eine geheime Fragestellung wie "Wie ist der Geburtsname deiner Mutter?" 
* Eigentums- bzw. Besitzfaktor - Ein Gegenstand
  * Führerschein, Personalausweis, EC-Karte, E-Mail-Adressen, Telefonnummern oder temporäre Schlüssel-Codes die über SMS-Tans, Sprachanrufe oder E-Mails versendet werden.
* Inhärenz- bzw. Existenzfaktor - Ein Merkmal
  * Fingerabdruck über den Sensor am Handy oder Laptop, Iris-Scanner

Diese Ein-Faktor-Authentifizierung reicht aber nicht mehr aus, da die meisten Dienste lediglich Username und Passwort gebrauchen.

### Zwei-Faktor-Authentifizierung

Wenn die Login-Daten herausgegeben wurden, fehlt dem Daten-Dieb immer noch ein einzigartiger Faktor zur Identifizierung.

Zu diesen zwei Faktoren zählt zum einen eine Kombination aus zwei der oben genannten Faktoren. Es werden aber noch einige andere Faktoren getestet und entwickelt. 

* Standortfaktor
  * Loggt sich ein Benutzer außerhalb seiner gewohnten Umgebung oder IP-Adresse ein, muss er einen zusätzlichen Schlüsselcode eingeben. 
* Verhaltensbasierende Faktoren
  * Surfverhalten, Stimme, Bewegung mit der Maus oder auf dem Touchscreen.

Am häufigsten werden aber Passcodes auf Mobilgeräte wie Smartphone und Mobiltelefon gesendet. Dieser Code ist nur einmal gültig und wird per E-Mail, SMS oder über eine entsprechende App versendet.

### Apps

* Google Authenticator
* Authy
* Microsoft Authenticator

## OAuth

OAuth ist ein offenes Protokoll, das API-Autorisierung für Desktop- Web- und Mobile-Anwendungen ermöglicht. Der Benutzer kann mithilfe des Protokolls einer Anwendung (Client oder Third-Party) den Zugriff auf seine Daten erlauben. Dabei wird das Weitergeben der Passwörter an Dritte vermieden. 

### Token

OAuth verwendet Tokens zur Autorisierung eines Zugriffs auf geschützte Ressourcen.

* **Access-Token**
  * Um auf die Daten auf dem Ressource Server zugreifen zu können, muss ein Access-Token vom Client übermittelt werden. Der Client kann die gewünschte Berechtigung beim Authorization Server anfragen und der Server teilt die gewährten Berechtigungen dem Client daraufhin mit. Der Access-Token ist nur für eine begrenzte Zeit gültig.
* **Refresh-Token**
  * Wenn ein Access-Token abgelaufen oder ungültig geworden ist kann man mit dem Refresh-Token einen neuen Access-Token beim Authorization Server anfragen. Dieser Token hat ebenfalls eine zeitliche Begrenzung. Der Token wird genau wie der Access-Token nach der Autorisierung an den Client gesendet. Da der Refresh-Token bereits die Autorisierung des Ressource Owners repräsentiert, muss für eine Neuanfrage keine weitere Autorisierung eingeholt werden. Damit wird die Lebenszeit der Access-Token auf wenige Minuten minimiert und somit die Sicherheit des Protokolls erhöht.

### Protokollfluss

![](img\Protocol-Flow.png)

1. Client fordert Autorisierung vom Ressource Owner an. Wird bevorzugt über einen Authorization Server durchgeführt.
2. Client enthält eine Autorisierungsgenehmigung. Dies erfolgt über eine von vier Autorisierungsgenehmigungen.
   1. Authorization Code
   2. Implicit 
   3. Ressource Owner Password Credentials 
   4. Client Credentials 
3. Client fordert Access-Token und nutzt dabei die Genehmigung aus Schritt 2.
4. Der Authorization Server authentisiert den Client und Prüft Genehmigung. Wenn dies erfolgreich ist wird der Access-Token versendet.
5. Der Client fragt die geschützten Daten vom Ressource Server an.
6. Ressource Server prüft den Access-Token und sendet, wenn gültig, die gewünschten Daten.

## JWT

#### Was ist JWT?

JSON Web Token (JWT) ist ein Standard, welcher einen kompakten Weg Informationen zwischen zwei Seiten als JSON Objekt zu verschicken. Die Informationen sind vertrauenswürdig, da sie vorher digital signiert werden. JWT können mit einem secret (**HMAC** Algorithmus) oder public/private Key signiert werden, mit **RSA** oder **ECDSA**. Durch das signieren wird der JSON-String in einen binären Zeichensatz kodiert. Um diesen versenden zu können wird er in einen **base64-String** kodiert.

#### Wofür benutzt man JWT?

* **Autorisierung:** Wenn ein Benutzer eingeloggt ist, werden alle nachfolgenden Request mit dem JWT versendet, welcher es dem User erlaubt Routen, Services und Ressourcen zu verwenden. 
* **Informationsaustausch:** JWTs können auch sehr gut verwendet werden um sicher Informationen zwischen zwei Seiten zu verschicken, da die JWTs signiert werden können. Damit kann man sicher gehen, dass die Informationen auch vom richtigen Ort kommen. 

#### Struktur

Ein JWT besteht aus drei Teilen: 

* Header
* Payload
* Signatur

Deshalb sieht ein JWT normalerweise so aus: xxxx.yyyy.zzzz

##### Header

Der Header besteht aus zwei Teilen: Den Typ des Tokens und den Algorithmus, welcher zum signieren benutzt wurde. 

##### Payload

Der Payload enthält die claims. Claims sind Statements über z.B. den User und zusätzliche Informationen. Es gibt drei Arten von claims:

* registered claims
  * Vordefinierte Claims, welche nicht notwendig sind aber empfohlen werden, um nützliche Informationen bereitzustellen. Einige davon sind: iss (Aussteller), exp (Ablaufzeit), Sub (Betreff), aud (Publikum) und andere.
* public claims
  * Diese werden vom Benutzer definiert. Aber um Kollisionen bei den Namen zu vermeiden, sollten sie bei der  [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) definiert werden oder als URI, welcher einen Kollisionsresistenten Namespace enthalten.
* private claims
  * Diese Claims enthalten die Informationen, die von beiden Seiten benutzt werden können und keine **registered** oder **public claims** sind.

Die Namen der Claims sollen normalerweise drei Zeichen lang sein, damit das JWT so kompakt, wie möglich ist.

##### Signatur

Um die Signatur zu erstellen braucht man: 

* kodierten Header
* kodierten Payload
* Secret
* Algorithmus, der im Header spezifiziert wurde

Mit dem HMAC SHA256 Algorithmus sieht die Signatur so aus:

```js
HMACSHA256(
base64UrlEncode(header) + "." +
base64UrlEncode(payload),
secret)
```

Die Signatur wird verwendet um sicherzustellen, dass die Nachricht nicht verändert wurde. Wenn man einen private Key verwendet kann man auch verifizieren ob der Absender auch der Richtige ist.

## OpenID

### Was ist OpenID?

OpenID ist ein Authentifizierungssystem, bei dem sich ein Benutzer nur einmal bei einem OpenID Provider mit Benutzernamen und Passwort anmelden muss, um mit Hilfe seiner OpenID sich bei allen das System unterstützenden Websites anmelden kann. 

Für die Anmeldung mit OpenID wird eine OpenID-Identität benötigt. Da die Software Open Source ist kann sie einfach auf dem eigenen Server installiert werden. Durche die einfache Einrichtung wird bei vielen Anbietern zum normalen Login auch eine OpenId Authentifizierung. 

OpenID hatd ie Form einer URL. Dabei wird der Benutzername als Subdomain des Anbieters verwendet. Das kann so aussehen: 

* benutzer.example.com
* example.com/benutzer

Wenn man nur auf eine OpenID Anmeldeverfahren setzt, kann man auf die "Passwort vergessen" verzichten und durch das nicht speichern der Benutzernamen und Passwörter entfällt auch der dafür nötige Sciherheitsaufwand, da er auf den OpenID-Anbieter verlagert wird. Nachteil ist aber, dass klassische Elemente, wie ein Benutzername häufig nicht verwendet werden können und deshalb eien vollständige Registrierung vorgezogen wird. 

### Nutzung

Wenn eine Website die Anmeldung mit OpenID unterstützt, kann man eine OpenID bereits bei der Registrierung angeben. Der Betreiber der Webseite kann nun neun Informationen vom OpenID-Anbieter erhalten, wenn der Benutzer dem zustimmt und die Informationen zuvor beim Anbieter hinterlegt wurden. 

Beim Anmeldeverfahren wird der Nutzer auf die  Anmeldeseite des OpenID-Anbieters geleitet. Dort erfolgt die Anmeldung. Zur Sicherheit wird eine weitere hinweisende Seite angezeigt, die bestätigt werden muss. Wenn die Anmeldung als vertrauenswürdig gekennzeichnet wird, wird der Nutzer bei einer erneuten Anmeldung auf die Bestätigungseite verzichtet. Nach der Anmeldung wird der Nutzer auf die eigentliche Webseite im angemeldeten Zustand umgeleitet. Bei der Anmeldung kann die Webseite die neun Informationen abfragen und ist somit immer auf dem neusten Stand, was den Vorteil hat, dass der Nutzer diese Daten nur noch bei dem OpenId-Anbiter pflegen muss. Der Benutzer kann auch einer Datenübertragung dauerhaft zustimmen, damit er dieser nicht immer bei jeder Anmeldung angeben muss. 

### Sicherheit

Für den Nutzer ist es einfacher eine Loginseite auf Echtheit zu überprüfen, da sich durch OpenID nur auf eine einzige Seite beschränkt. Der Nutzer muss sich also nur die sicherheitsrelevanten Merkmale von einer Seite merken. Die OpenID-Provider sorgen ebenfalls für Sicherheit, indem sie:

* Cookies verwenden
* Ein individuelles Bild anzeigen 
* Den HTTP-Referer mit der IP des Requesters vergleichen 
* Clientseitiges TLS-Zertifikat zur Autentifizierung nutzen

## Hashfunktionen

Eine Hashfunktion bildet eine große Menge auf eine kleine Zielmenge ab. Die Eingabemenge kann Elemente unterschiedlicher Länge enthalten. Die Elemente der Zielmenge haben meist eine feste Länge. Die Werte der Hashes sind meistens eine Teilmenge der Ganzen Zahlen. 

Gute Hashfunktionen geben eine erwartete Eingabedaten Menge mit, damit zwei unterschiedliche Eingabedaten auch unterschiedliche Hash-Werte zugeordnet werden. Ein Kollision tritt dann auf, wenn ein Hashwert zwei Eingabedaten zugeordnet werden. Diese sind unvermeidlich da die Menge der Hash-Werte meist kleiner ist als die Menge der Eingabedaten. Daher gibt es Verfahren zur Erkennung von Kollisionen. Eine gute Hashfunktion erzeugt wenige Kollisionen. 

## Base64

### Was ist Base64?

Base64 ist eine Kodierung die 8-Bit-Binärdateien in einen Zeichensatz aus 64 Zeichen übersetzt. Sie wird verwendet um binäre Dateien versendbar zu machen. Hauptsächlich wird Base64 eingesetzt um den Email Anhang, wie Bilder zu kodieren. 

Der Zeichensatz kann aus folgenden Zeichen bestehen: 

* A-Z 
* a-z
* 0-9
* +
* /
* = (Als Identifier für Füllbits)

### Wie wird kodiert?

Es werden jeweils drei Bytes der Binärdatei in vier 6-Bits aufgeteilt. Diese Blöcke bilden jeweils eine Zahl von 0-63. Diese Zahlen können einem der 64 Zeichen, aus dem der Base64-String bestehen kann, zugeordnet.

![](img\Base64-Tabelle.JPG)

Ist die Länge des Bytesatzes nicht durch drei teilbar, wird der String mit aus Nullbits bestehenden Füllbytes aufgefüllt. Diese werden am Ende des Strings mit 0-2 =-Zeichen kodiert. Dies bedeutet, dass entweder kein, ein oder zwei Bytes mit Füllbytes aufgefüllt wurden. 

### Große eines Base64-Strings

Eine n-Byte große Binärdatei wird in einen 4 * ( n / 3 ) Byte großen Base64-String kodiert.``

## Quellen

* https://www.welivesecurity.com/deutsch/2016/05/04/grundlagen-der-authentifizierung/
* https://de.wikipedia.org/wiki/Zwei-Faktor-Authentisierung
* https://de.wikipedia.org/wiki/OAuth
* https://www.oauth.com/oauth2-servers/getting-ready/
* https://www.ibm.com/support/knowledgecenter/de/SS7K4U_liberty/com.ibm.websphere.wlp.zseries.doc/ae/cwlp_authentication.html#cwlp_authentication
* https://www.techfak.uni-bielefeld.de/~walter/vpn/ch08s04.html

