# Wie wir das Repo managen

## Änderungen in den Master bekommen

Änderungen dürfen nur über einen PR in das Repo und auch nur wenn mindestens eine weitere Person einen kontrollblick getätigt hat. Wir hatten ein unentschieden der Stimmen welche Variante wir nutzen wollen 7 Stimmen für jeder hat einen Branch auf den Repo und ebenfalls 7 Stimmen für jeder erstellt einen Fork. Einige von euch haben daraufhin diese Entscheidung dem Repo Owner überlassen. Somit werden wir das Projekt Forken und wenn ihr Änderungen abgeschlossen habt oder veröffentlichen wollt stellt ihr einen PR an dieses Hauptrepo.

Der Master Branch des Hauptrepos ist nur gegen pushen gesperrt und nicht gegen klonen/pullen. Somit könnt ihr euer Repo jederzeit aktualisieren.

## Projekttagebuch

### Markdown Datei

Euer Tagebuch soll wie folgt benannt werden 

vornamenachname.md

der Ort wo ihr die Datei abspeichern sollt liegt unter docs/devdiaries

ein verlinken wird vom Repo owner getätigt somit ist keine Änderung an der Sidebar von Nöten dies ist um eine gleichmäßige Sortierung einzubringen.

### Struktur

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

als nächstes kommt die Struktur eurer Einträge eine Tabelle welche folgenden Grundinformationen benötigt: Wann wurde welche Aktivität für wie lange gemacht und gab es Probleme oder gab es etwas was besonders gut funktioniert hat.

```markdown
| Datum      | Tätigkeit           | Dauer  | Zusatz |
| ---------- | ------------------- | ------ | ------ |
| 09.04.2019 | Beispiel entwickelt | 10 min |        |

```

## Thema

Struktur

```
project
└───docs
    └───themaname
        │   index.md
        └───img
```

Wie Ihr eure Themen index.md befüllt ist euch überlassen. Eine große Aufgabe ist es ja auch eine Ausarbeitung eures Themas zu erfüllen, da ein rein Theoretisches Thema anders ist als ein Framework Thema. 

Ihr müsst ebenfalls nichts an der Sidebar.md ändern dies fällt wieder unter den Aufgabenbereiches des Repo owners.

