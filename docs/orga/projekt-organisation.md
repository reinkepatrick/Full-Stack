
## Wie wir das Repo managen

### Änderungen in den Master bekommen

Änderungen dürfen nur über einen PR in das Repo und auch nur wenn mindestens eine weitere Person einen Kontrollblick getätigt hat. Wir hatten ein unentschieden der Stimmen welche Variante wir nutzen wollen. 

Sieben Stimmen für jeder hat einen Branch auf den Repo und ebenfalls Siebn Stimmen für jeder erstellt einen Fork. Einige von euch haben daraufhin diese Entscheidung dem Repo Owner überlassen. Somit werden wir das Repository forken und wenn ihr Änderungen abgeschlossen habt oder veröffentlichen wollt, stellt ihr einen PR an das .

Der Master Branch des Hauptrepos ist nur gegen pushen gesperrt und nicht gegen klonen/pullen. Somit könnt ihr euer Repo jederzeit aktualisieren.


### Projekttagebuch

#### Markdown Datei

Euer Tagebuch soll wie folgt benannt werden 

```
vornamenachname.md
```

der Ort, wo ihr die Datei abspeichern sollt, liegt unter dem Ordner  *docs/devdiaries*.

Ein verlinken wird vom Repo owner getätigt somit ist keine Änderung an der Sidebar vonnöten. Dies ist um eine gleichmäßige Sortierung einzubringen.


#### Struktur

Header 1 wo ihr euren Vornamen Nachnamen - Projekttagebuch

```markdown
# Alexander Heinisch - Projekttagebuch
```

darauf für jede Woche einen Header 2 mit der Wochen nr.

```markdown
## Woche 1
```

hinter diesem Header soll der start der Woche sowie das Ende der Woche stehen in fett 

```markdown
__08.04.2019 - 14.04.2019__
```

als nächstes kommt die Struktur eurer Einträge. Eine Tabelle, welche folgende Grundinformationen benötigt:  
 - Wann wurde die Aktivität gemacht?
 - Was war dies für eine Aktivität?
 - Wie lange wurde benötigt für diese Aktivität?
 - Gab es Probleme oder etwas was sehr gut funktioniert hat?

```markdown
| Datum      | Tätigkeit           | Dauer  | Zusatz |
| ---------- | ------------------- | ------ | ------ |
| 09.04.2019 | Beispiel entwickelt | 10 min |        |

```

### Thema

Struktur

```
project
└───docs
    └───themaname
        │   index.md
        └───img
```

Wie Ihr eure Themen index.md befüllt ist euch überlassen. Eine große Aufgabe ist es ja auch eine Ausarbeitung eures Themas zu erfüllen, da ein rein theoretisches Thema anders ist als ein Framework Thema. 

Ihr müsst ebenfalls nichts an der _Sidebar.md ändern, da dies wieder unter den Aufgabenbereiches des Repo owners fällt.
