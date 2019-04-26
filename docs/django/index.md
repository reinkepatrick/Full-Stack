# Django

## Was ist Django?
Django ist ein Python Framework für Backends von Web Applications.

Django ist im Release 2.2 und wird von Python 3.5 und folgende unterstützt.

- Läuft auf Apache Web Server (Produktion). Hat einen eigenen Webserver für Tests
- MTV (Model Template View) als Kern Architektur
- Object Relational Mapper
- Python unit test framework
- Unterstützung von bekannten Datenbanken
- Platformübergreifend
  
### Bekannte Firmen

- Spotify
- Dropbox
- Mozilla
- NASA
- Pinterest
- Reddit

### Django Struktur
![MTV](./img/MTV-Diagram.png ':size=525')

## Workflow
Als erstes wird Python 3.x.y installiert. Danach kann noch eine Datenbank (Maria DB) einrichten.

Nachdem dann Django installiert wurde, kann man sein Projekt erstellen.

```
$ django-admin startproject testproject
```

### Shell
In der Python Shell vom Projekt, können zum Testen, Befehle ausgeführt werden.
```bash
$ python manage.py shell
```

## Apps
Ein Projekt beinhaltet verschiedene Apps welche verschiedene Aufgaben in einem Projekt übernehmen.
Beispiel: Forum beinhaltet verschiedene Apps
- User login
- Notifications
- newsletters
- polls
- ...

Diese Apps können wiederverwendet werden. Zum erstellen einer App einfach den Befehl ausführen

```
$ python manage.py startapp appname
```

### Einbinden von Apps im Projekt
Die App muss noch in das Projekt eingebunden werden. Dazu muss im Projekt die Datei settings.py angepasst werden. Es wird ein Attribut im Array _INSTALLED_APPS_ eingefügt.

Die Klasse `TestappConfig` ist in der Datei `testapp/apps.py`.

```python
INSTALLED_APPS = [
    'testapp.apps.TestappConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```


## Views
Views sind für die Trennung zwischen Logik und Frontend zuständig. Jede App besitzt eine View. Die einfachste View ist diese hier:

```python
# testapp/view.py
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the test index.")
```

Wenn von dieser View nun die Methode `index` aufgerufen wird, sendet die View einen HttpResponse mit der Nachricht zurück.  
Damit die View aufgerufen werden kann, müssen die [URLs](#URLs) konfiguriert werden. 

Views müssen am Ende einen HTTP Response haben oder eine Exception wie HTTP 404.

## URLs
Es gibt eine URL Konfigurationsdatei, welche beim Projekt anlegen existiert. Dies ist die Root Konfigurationsdatei. In jeder App wird noch eine weitere URL Datei angelegt. In der Root Konfigurationsdatei wird zu der URL Datei von einer App weitergeleitet. Dazu wird ein Array `urlpatterns` erstellt, indem die verschiedenen Pfade zu den Apps angegeben werden. Von der URL Datei in der App wird zu den Methoden von der Views Datei geleitet.

```python
# test_project/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('test/', include('testapp.urls')), # Modul von testapp includen.
    path('admin/', admin.site.urls), # Von Django automatisch generiert
]
```

```python
# testapp/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'), # Aufruf der Methode index in testapp/views.py
]
```
Die Methode `path` kriegt als ersten Parameter das [URL Pattern](#URL_Pattern), welche aufgerufen werden soll. Als zweiter Parameter wird die Methode übergeben, welche aufgerufen wird und mit dem Parameter `name` kann der URL einen Namen gegeben werden, um darauf später referenzieren zu können.

Nun muss noch die root URL konfiguriert werden. Damit dies weiter zur `testapp` geleitet wird.

### URL_Pattern
URL Patterns können Reguläre Ausdrücke sein. Außerdem kann noch bestimmt werden, ob der übertragene Wert ein Parameter für eine View ist oder nicht.  
Beispiel:
```python
path('testapp/<int:question_id>/vote', views.votes, name=vote)
```
In dem Beispiel wird der übergebene Parameter nach `testapp/` als Parameter für die Methode `views.votes` gesehen. Der Parameter ist ein Int und der Name des Parameters ist `question_id`.

Bei einem regulären Ausdruck sieht das ganze dann so aus.

```python
re_path(r'(?P<question_id>\d+)/results/', views.results, name=results)
```
In dem Beispiel muss im Gegensatz zu den anderen URL Patterns die Methode `re_path()` verwendet werden.

## Models
Models sind die Schnittstelle von Django und der Datenbank.
Jede App hat dabei eine eigene Datei `models.py`, in der die Schnittstelle implementiert wird. Für eine Polls App sieht die Datei `models.py` so aus.

```python
# testapp/models.py
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

Jede Klasse in der Datei `models.py` steht für eine Tabelle. Dann werden die Attribute der Tabelle festegelegt.

## Migrieren der Datenbank
Damit der Code in die Datenbank migriert wird, muss der Befehl `makemigrations` ausgeführt werden. Damit werden die Änderungen der Datenbank aufgeschrieben und im Ordner `testapp/migrations/` gespeichert.

```bash
$ python manage.py makemigrations
```

Die Änderungen wurden aufgeschrieben, nun können die Änderungen mit der Methode `migrate` auf die Datenbank ausgeführt werden. Die Methode `migrate` führt für alle Änderungen, die in den Ordnern `app/migrations/` gefunden wird, durch.

```bash
$ python manage.py migrate
```

Damit wurden die Tabellen in der Datenbank erstellt.

## Datenbank API

Befinden wir uns in der [Python Shell](#Shell), können wir die Datenbank API anwenden. Dabei werden die zuvor erstellten [Models](#Models) verwendet. Als erstes müssen die Klassen aus `testapp/models.py` importiert werden.

```python
>>> from testapp.models import Choice, Question
```
<br></br>

### Datenzeile erstellen
Nun wird das erste Objekt für die Tabelle `Question` erzeugt. Mit der Methode `save()`, wird dies dann in die Datenbank geschrieben.

```python
>>> from django.utils import timezone
>>> q = Question(question_text="First Question", publication_date=timezone.now())
>>> q.save()
```
<br></br>

### Datenzeile für Fremdschlüsselbeziehung erstellen
Um eine Datenzeile für Fremdschlüsselbeziehungen zu bekommen, kann auf das `set` Attribut von `Question` zugegriffen werden. Diese werden automatisch von Django erstellt.
```python
>>> q.choice_set.create(choice_text='First Choice', votes=0)
>>> q.choice_set.create(choice_text='Second Choice', votes=0)
>>> q.choice_set.create(choice_text='Third Choice', votes=0)
```
<br></br>

### Datenzeile ändern
Zum Ändern muss nur das Attribut des Objekts geändert werden und dann wieder in der Datenbank gespeichert werden. 

```python
>>> q.question_text = "New Question"
>>> q.save()
```
<br></br>

### Alle Datenzeilen bekommen
Mit der Methode `all()` können alle Datenzeilen aus einer Tabelle abgefragt werden. Die Daten aus der Datenbank werden als `<QuerySet[]>` gespeichert.
```python
>>> Question.objects.all()
<QuerySet[ Question: First Question ]>
```
<br></br>

### Bestimmte Datenzeilen bekommen
Mit der Methode `filter()` kann bei der Query nach bestimmten Abfragen gefiltert werden. Welche Möglichkeiten es dabei gibt, ist in der [Dokumentation](https://docs.djangoproject.com/en/2.2/ref/models/querysets/#id4) von Django beschrieben.  
Als Beispiel, eine Abfrage zum Abfragen von allen `Question` Datenzeilen, dessen ID größer als 14 ist. Beispiel:
```python
>>> Question.objects.filter(pk__gt=14)
```

Mit der Methode `get()` wird nur eine Datenzeile als Objekt zurückgegeben. Im Beispiel wird das Objekt mit dem Schlüssel 1 zurückgegeben und in die Variable `q` gespeichert.
```python
>>> q = Question.objects.filter(pk=1) # Äquivalent zu Question.objects.filter(id=1)
```
<br></br>


### Datenzeilen von Fremdschlüsselbeziehungen bekommen
```python
>>> q = Question.objects.filter(pk=1)
>>> q.choice_set.all()
```
<br></br>

### Löschen von Datenzeilen
Löschen einer Datenzeile ist mit der Methode `delete()` möglich. Der Aufruf der Methode `save()` ist nicht notwendig.
```python
>>> question = Question.objects.filter(pk=1)
>>> question.delete()
```

## Templates

