# Sicherheit und Authentifizierung 

## Authentifizierung

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

## Einrichten von OAuth

#### Vorab:

Um eine Anwendung für OAuth zu erstellen muss man diese registrieren nachdem man sich als Entwickler beim Service angemeldet hat. Dabei muss man folgendes angeben: 

* Name 
* Website 
* Logo 
* etc.

Wenn die Registrierung abgeschlossen ist, bekommt man eine **client_id** und in manchen Fällen auch ein **client_secret.**  Zudem sollte man eine oder mehrere **redirect-url** angeben. Dort werden nach erfolgreicher Autorisierung hingeleitet. Auf folgendes sollte man, um die Sicherheit zu erhöhen, achten:

* Auf Querys verzichten
* https verwenden 

Um Querys zu vermeiden kann man auf **states** zurückgreifen. Diese sind einfache Strings, die bei der Autorisierung übergeben werden. 

#### Login mit Google 

##### Registrierung

OAuth kann man auch verwenden um sich mit Google zu autorisieren. Zu aller erst muss man bei der Google API Console eine Anwendung erstellen um eine **client_id**  und **client_secret** zu bekommen. Außerdem muss man noch eine **redirect URL** registrieren. 

<https://console.developers.google.com/>

![](img\google-create-credentials.png)

##### Erstellung der Anwendung

Als nächstes wird die Anwendung geschrieben über die man sich mit Google authentifizieren kann. Diese wird als Beispiel in php geschrieben.

```php

$googleClientID = '';
$googleClientSecret = '';
 
$authorizeURL = 'https://accounts.google.com/o/oauth2/v2/auth';
 
$tokenURL = 'https://www.googleapis.com/oauth2/v4/token';
 
$baseURL = 'https://' . $_SERVER['SERVER_NAME']
    . $_SERVER['PHP_SELF'];
 
session_start();
 
```

Mit diesen Variablen definiert kann eine Session gestartet werden. Um zu zeigen das es funktioniert, wird eine einfache Seite geschrieben die zeigt ob der User ein- oder ausgeloggt ist. 

```php
// Wenn schon eine user_id existiert ist der User bereits eingeloggt
if(!isset($_GET['action'])) {
  if(!empty($_SESSION['user_id'])) {
    echo '<h3>Logged In</h3>';
    echo '<p>User ID: '.$_SESSION['user_id'].'</p>';
    echo '<p>Email: '.$_SESSION['email'].'</p>';
    echo '<p><a href="?action=logout">Log Out</a></p>';
 
    // Userinfo von Google 
    echo '<h3>User Info</h3>';
    echo '<pre>';
    $ch = curl_init('https://www.googleapis.com/oauth2/v3/userinfo');
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
      'Authorization: Bearer '.$_SESSION['access_token']
    ]);
    curl_exec($ch);
    echo '</pre>';
 
  } else {
    echo '<h3>Not logged in</h3>';
    echo '<p><a href="?action=login">Log In</a></p>';
  }
  die();
}
```

##### Autorisierungs Prozess

Als erstes müssen die User mit der Query ?action=login in der URL auf die Seite geleitet werden. Das startet den Prozess.

```php
// Startet den Prozess indem der User auf Google's Autorisierungs Seite geleitet wird
if(isset($_GET['action']) && $_GET['action'] == 'login') {
  unset($_SESSION['user_id']);
 
  // Generiert zufälligen Hash und speichert ihn in Session
  $_SESSION['state'] = bin2hex(random_bytes(16));
 
  $params = array(
    'response_type' => 'code',
    'client_id' => $googleClientID,
    'redirect_uri' => $baseURL,
      // Zeigt an, dass man nur wissen möchte wer die Person ist und nicht auf Google Daten zugreifen möchte
    'scope' => 'openid email',
    'state' => $_SESSION['state']
  );
 
  // Redirect the user to Google's authorization page
  header('Location: '.$authorizeURL.'?'.http_build_query($params));
  die();
}
```

Wichtig ist es eine **state** zu benutzen um den Client zu schützen. Er ist ein zufälliger String, der vom Client generiert wird und in der Session gespeichert wird. Mit diesem Parameter verifiziert der Client die Antwort, die Google dem User zurückschickt.  

Es wird eine URL erstellt, die die **client_id**,die **redirect_URL**, was wir anfragen (**scope**) und dem **state**. 

Wenn ein User einen Account ausgewählt hat, wird er wieder auf die Seite zurückgeleitet mit den Parametern: 

* code
* state

##### Verarbeitung der Antwort

```php
//Prüfen ob code und state in der query vorhanden sind
if(isset($_GET['code'])) {
  // Verify the state matches our stored state
  if(!isset($_GET['state']) || $_SESSION['state'] != $_GET['state']) {
    header('Location: ' . $baseURL . '?error=invalid_state');
    die();
  }
 
  // Verifizierung des Codes
  $ch = curl_init($tokenURL);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query([
    'grant_type' => 'authorization_code',
    'client_id' => $googleClientID,
    'client_secret' => $googleClientSecret,
    'redirect_uri' => $baseURL,
    'code' => $_GET['code']
  ]));
  $response = json_decode(curl_exec($ch), true);

    //Code aus dem nächsten Block ...
}
```

Zuerst wird überprüft ob der Parameter **state** in der Antwort mit dem **state** in der Session übereinstimmt. Danach wird ein POST request an Google gesendet mit:

* client_id
* client_secret
* Autorisierungs Code

Dieser Request wird von Google verifiziert und antwortet mit einem **acces token** und einem **ID token**, welche ähnlich wie in der Abbildung unten aussieht.

```php
{
  "access_token": "ya29.Glins-oLtuljNVfthQU2bpJVJPTu",
  "token_type": "Bearer",
  "expires_in": 3600,
  "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImFmZmM2MjkwN
  2E0NDYxODJhZGMxZmE0ZTgxZmRiYTYzMTBkY2U2M2YifQ.eyJhenAi
  OiIyNzIxOTYwNjkxNzMtZm81ZWI0MXQzbmR1cTZ1ZXRkc2pkdWdzZX
  V0ZnBtc3QuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQi
  OiIyNzIxOTYwNjkxNzMtZm81ZWI0MXQzbmR1cTZ1ZXRkc2pkdWdzZX
  V0ZnBtc3QuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIi
  OiIxMTc4NDc5MTI4NzU5MTM5MDU0OTMiLCJlbWFpbCI6ImFhcm9uLn
  BhcmVja2lAZ21haWwuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUs
  ImF0X2hhc2giOiJpRVljNDBUR0luUkhoVEJidWRncEpRIiwiZXhwIj
  oxNTI0NTk5MDU2LCJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2ds
  ZS5jb20iLCJpYXQiOjE1MjQ1OTU0NTZ9.ho2czp_1JWsglJ9jN8gCg
  WfxDi2gY4X5-QcT56RUGkgh5BJaaWdlrRhhN_eNuJyN3HRPhvVA_KJ
  Vy1tMltTVd2OQ6VkxgBNfBsThG_zLPZriw7a1lANblarwxLZID4fXD
  YG-O8U-gw4xb-NIsOzx6xsxRBdfKKniavuEg56Sd3eKYyqrMA0DWnI
  agqLiKE6kpZkaGImIpLcIxJPF0-yeJTMt_p1NoJF7uguHHLYr6752h
  qppnBpMjFL2YMDVeg3jl1y5DeSKNPh6cZ8H2p4Xb2UIrJguGbQHVIJ
  vtm_AspRjrmaTUQKrzXDRCfDROSUU-h7XKIWRrEd2-W9UkV5oCg"
}
```

Das **access token** wird von der Anwendung nur verwendet um einen API request zu machen. Das **id token** ist ein JWT und enthält die Informationen über den User, der sich eingeloggt hat.

##### Verifizierung der User Info

Der **id token** ist ein drei Teile aufgeteilt, welche alle mit einem Punkt geteilt werden. Der mittlere Part ist ein base-64 kodierter JSON String  und enthält die Daten des **id tokens**.

```php
{
  "azp": "272196069173.apps.googleusercontent.com",
  "aud": "272196069173.apps.googleusercontent.com",
  "sub": "110248495921238986420",
  "hd": "okta.com",
  "email": "aaron.parecki@okta.com",
  "email_verified": true,
  "at_hash": "0bzSP5g7IfV3HXoLwYS3Lg",
  "exp": 1524601669,
  "iss": "https://accounts.google.com",
  "iat": 1524598069
}
```

Wichtig hier sind die Einträge **email** und **sub**. **Sub** enthält einen einzigartigen Identifier des eingeloggten Users. Diese Teile werden zusammen mit dem **access token** und dem **id token** in der Session gespeichert.

```php
// ... Code aus dem vorherigen Block
 
  // JWT in drei Teile teilen
  $jwt = explode('.', $data['id_token']);
 
  // Mittleren Teil base64 und JSON dekodieren
  $userinfo = json_decode(base64_decode($jwt[1]), true);
 
  $_SESSION['user_id'] = $userinfo['sub'];
  $_SESSION['email'] = $userinfo['email'];
 
  // Speichern des access und id tokens in der session
  $_SESSION['access_token'] = $data['access_token'];
  $_SESSION['id_token'] = $data['id_token'];
 
  header('Location: ' . $baseURL);
  die();
}
```

Danach kann man wieder auf die Homepage der Anwendung zurückgeleitet werden, wo jetzt die **userID** und die **email** angezeigt werden.

```php
echo '<p>User ID: '.$_SESSION['user_id'].'</p>';
echo` `'<p>Email: '.$_SESSION['email'].'</p>';
```

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
* https://www.ibm.com/support/knowledgecenter/de/SS7K4U_liberty/com.ibm.websphere.wlp.zseries.doc/ae/cwlp_authentication.html#cwlp_authentication__OpenID