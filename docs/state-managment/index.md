# Einleitung

## Was ist "State Managment"?

State Managment umfasst die Verwaltung von verschiedenen Zuständen eines Objektes innnerhalb dieses Objektes. 



## Wo finde ich State Managment?

State Managment finden wir in fast allen modernen Softwaresystemen. Dabei ist es egal, ob wir gerade ein soziales Netzwerk bedienen, welches uns immer die aktuelle Anzahl ungelesener Nachrichten mitteilt, oder einen Online Shop, wo wir immer wissen wollen, wie viele Waren gerade im Einkaufswagen abgelegt sind. Aber auch Desktop Apps nutzen State Managment. So wäre es Spotify zum Beispiel schwierig, immer das aktuelle Lied, welches gerade abgespielt wird, auf jeder Unterseite anzuzeigen.



## Gründe für State Managment

Der größte Grund für State Managment ist die Verbesserung des Usererlebnisses. Nehmen wir als Beispiel die Spotify App. 

![Beispiel Spotify](img/BeispielSpotify.png)

In dem unteren Bereich sieht man welches Lied momentan gespielt wird. Dazu werden noch Zusatsinformationen eingeblendet wie der Name des Künstlers und das Musikalbum. Rechts sieht man eine Fortschrittsleiste für das Lied und noch weiter rechts, in diesem Bildausschnitt nicht vorhanden, sind auch die üblichen Steuerungsbutton zum Abspielen von Musik. In dem oberen, dunkleren Bereich sehe ich die Komponente die ich gerade geöffnet habe. In diesem Beispiel handelt es sich um die verschiedenen Titel in einer Playlist.

Während der obere Bereich immer wechselt, je nachdem welche Komponente gerade geöffnet wurde, bleibt der untere an sich immer gleich. 

Doch was sind die Effekte in diesem Beispiel von schlechtem oder gar keinem State Managment? Der Aufbau der App würde sich nicht zwingend verändern. Aber das Benutzererlebnis könnte sich signifikant verschlechtern. Man könnte sich zum Beispiel vorstellen, dass nicht mehr immer der aktuelle Titel angezeigt wird, sondern vielleicht teilweise ein bereits gespielter. Der User würde bei solchen Nachteilen schnell genervt sein und womöglich sogar auf ein Konkurrenzprodukt umsteigen.

Entstehen könnten solche Fehler durch eine inkonsistente Behandlung der Zustandswerte. Wenn zwei verschiedene Komponenten auf unterschiedliche Daten zugreifen um das aktuelle Lied zu erfahren, muss gewährleistet werden, dass beide Daten immer aktuell sind. Viel einfacher ist es natürlich, wenn jede Komponente den Zustand zentral erfragen kann. 

Neben der Konsistenz der Zustände in den verschiedenen Komponenten und der damit einhergehende Verbesserung der Nutzbarkeit gibt es auch noch weitere positive Effekte. So ist der Code zum Beispiel wartbarer und besser erweiterbar. Die Pflege der Zustände erfolgt zentral, weshalb ich auch nur an einer Stelle Änderungen oder Erweiterungen pflegen muss. 

# Grundlagen

## Definitionen

### State / Zustand

Wird häufig durch die Gesamtheit der Attribute und ihrer Werte beschrieben. Der Zustand ergibt sich also aus der momentanen Belegung aller Attribute. 



## Zustandsdiagramm

Um die verschiedenen Zustände darzustellen, wird meistens ein Zustandsdiagramm verwendet.



![Zustandsdiagramm Beispiel](img/StatusBeispielSpotify.png)

In dem Diagramm sind nur zwei Zustände und deren Übergänge dargestellt. Die beiden Zustände sind, dass entweder die Musik spielt oder stoppt. Ein Wechsel in andere Zustände wird durch die beiden Methoden Play() oder Stop() realisiert.



Hierbei handelt es sich natürlich um ein sehr einfaches Beispiel. Größere Anwendungen werden allerdings viele verschiedene Zustände haben. Die Fälle und Übergänge werden also deutliche komplexer. 

## Historie

Wie hat man das früher gelöst? In den 1970er Jahren war strukturierte Programmierung der Maßstab und man hatte noch keine Verhaltensmuster. Man hätte für diesen Fall jedem Zustand einen Integer zugeteilt (Fröhlich = 0, Neutral =1, Bockig = 2) und hätte anschließend verschiedene Fallunterscheidungen ausgeführt. In Pseudocode würde das für die Methode verärgern() ungefähr so aussehen:

```java
switch aktuellerZustand:
// Zustandsabhängiges Verhalten
case FRÖHLICH:
// Verhalten wenn Fröhlich
​	aktuellerZustand = BOCKIG;

case NEUTRAL:
// Verhalten wenn Neutral
​	aktuellerZustand = BOCKIG;

case BOCKIG: 
// Verhalten wenn Bockig
​	aktuellerZustand = BOCKIG;
```

Wir können uns jetzt ohne Probleme vorstellen, dass diese Art der Lösung sehr unübersichtlich wird, wenn man mehrere Zustände hat. Häufig kommen dann auch noch weitere Verschachtelungen dazu, wirklich handelbar ist das Ganze also nicht mehr. Auch die Wartbarkeit leidet stark unter diesem Entwurf. Hat die Freundin jetzt noch einen weiteren Zustand, so müsste jede Methode einzeln angepasst werden. Dies ist viel zu aufwändig und wahrscheinlich würde man welche vergessen.

# Quellen

<https://de.wikipedia.org/wiki/Zustand_(Entwurfsmuster)#Beispiele>



<https://www.philipphauer.de/study/se/design-pattern/state.php>



<https://blogs.itemis.com/de/modellieren-mit-zustandsautomaten-teil-5-das-state-pattern>



<https://de.wikipedia.org/wiki/Zustand_(Entwurfsmuster)>



Theoretische Grundlagen der Informatik, Ralf Socher, ISBN 978-3-446-41260-6



Der C++-Programmierer, Ulrich, Breyman, ISBN 978-3-446-44346-4





