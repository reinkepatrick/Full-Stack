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

### HTTP Response

Die Klasse `HttpResponse` hat folgende Eigenschaften:
* `status_code`
* `content`
* `closed`
* `reason_phrase`

Wenn im Response noch weitere Parameter übergeben werden, können auch auf diese zugegriffen werden. Ein typischer Parameter ist `context`. Damit übergibt die View Objekte.

### Generic Views
Generic Views sind dafür da, um Wiederholungen zu vermeiden und weniger Code zu schreiben. Es gibt zwei verschiedene Generic Views:
* ListView
* DetailView

Bei der `ListView` wird mit einer Liste von Objekten gearbeitet und bei der `DetailView` nur mit einem Objekt.  
Eine Generic View muss ein Model mitgegeben bekommen.  
Eine `DetailView` erwartet den Hauptschlüssel als Parameter, weshalb dies in der [URL](#urls) geändert werden muss.

In den Generic Views, muss auch der `template_name` gesetzt werden.  

Der übergebene Kontext für das [Template](#templates) ist bei der `DetailView` das Model und beim `ListView` das Attribut `context_object_name`

Beispiel __DetailView__:

```python
class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'
```
Das Model enthält nun die Question mit dem übergebenen Hauptschlüssel. Dies wird automatisch an das Template übergeben, da eine GenericView bestimmte Methoden abarbeitet.

1. setup()
2. dispatch()
3. http_method_not_allowed()
4. get_template_names()
5. get_slug_field()
6. get_queryset()
7. get_object()
8. get_context_object_name()
9. get_context_data()
10. get()
11. render_to_response()

Die Methoden können überschrieben werden.

Beispiel __ListView__:

```python
class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]
```

Eine ListView brauch keinen übergebenen Parameter, jedoch muss dann die Methode `get_queryset` überschrieben werden.
Die abgearbeiteten Methoden sind. 

1. setup()
2. dispatch()
3. http_method_not_allowed()
4. get_template_names()
5. get_queryset()
6. get_context_object_name()
7. get_context_data()
8. get()
9. render_to_response()

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

## Django Admin
Mit Django hat man die Möglichkeit, eine Admin Seite zu betreten. Dort können z.B. die Datenbank Einträge eingesehen und geändert werden. Um einen Admin zu erstellen, wird dieser Befehl auf der Shell ausgeführt.

```bash
$ python manage.py createsuperuser
```

Danach wird man auch nach Username und Passwort gefragt. Nun kann der Benutzer sich unter der URL `/admin/` anmelden. Damit die Models auf der Admin Seite sichbar sind, muss dies noch in der Datei `testapp/admin.py` der App, angepasst werden.

```python
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

## Templates
Templates sind dafür da, um beim Response ein Design für bestimmte Responses zu ermöglichen. Somit werden Views von der Benutzeroberfläche abgekapselt.  
Die Templates sind in einer HTML Datei geschrieben, welche dann in der View angegeben werden muss. Im Template selber können auf Variablen zugegriffen werden. Dies ist durch Django möglich. Genauso können auch Kontrollstrukturen verwendet werden.

Beispiel Ablauf:
1. Benutzer ruft URL auf.
2. URL leitet weiter zur Methode in View.
3. View bekommt Daten vom Model.
4. Daten werden zum Template gegeben, welches Daten anzeigt.

## Tests

Tests werden üblicherweise in einer Datei `tests.py` geschrieben. Diese Datei befindet sich in der jeweiligen App. 

Tests werden folgendermaßen ausgeführt.
```bash
$ python manage.py test <appname>
```

Wenn ein Test ausgeführt wird, werden folgende Schritte ausgeführt.
* `manage.py` sucht nach Tests in den angegebenen apps.
* Wenn in einer Datei die Subklasse django.test.TestCase vorhanden ist, wird diese Datei als Testfile anerkannt.
* Für die Tests wird eine Test Datenbank erstellt.
* Es wird nach Methoden gesucht, welche mit __test__ anfangen.
* Die Methode wird ausgeführt.
* Nachdem alle Tests durch gelaufen sind, wird die Datenbank gelöscht.

### Views
Für Tests der Views gibt es eine Klasse `Client`, welche den Client simulieren kann und somit mit den Code der View interagiert.
Zunächst musss eine Testumgebung erstellt werden. 
```python
from django.test.utils import setup_test_environment
setup_test_environment()
```

Nun kann mit der Client Klasse auf URLs zugegriffen werden. Beispiel:  
```python
from django.test import Client
client = Client()
response = client.get('/')
```

Der Response ist ein [HTTP Response](#http-response)
Darauf können dann die Tests ausgeführt werden.


## Channels
Channels ist eine Library für Django Projekte. Da Django üblicherweise nur mit HTTP Requests kommuniziert, ermöglicht es Channels, dass Websockets verwendet werden können, ohne dabei auf die Funktionen von Django selbst zu verzichten.

### Wie funktionierts?

![MTV](./img/django-wsgi.png ':size=525')

### Konfiguration

Damit Channels verwendet werden kann, müssen bestimmte Konfigurationen durchgeführt werden. 

In der `settings.py` muss in dem Array `INSTALLED_APPS` die App `channels` hinzugefügt werden. Ebenso muss angegeben werden, dass Django nun mit Websockets angesprochen wird und nicht mit HTTP Requests. Dies kann mit dieser Zeile in den Settings beschrieben werden.
```python 
ASGI_APPLICATION = "myproject.routing.application"
```

Nun muss noch in der root App eine Datei `routing.py` erstellt werden. Diese Datei ist das äquivalent zu den `urls.py` Dateien. Hier wird beschrieben, welcher Consumer ausgewählt werden soll, wenn eine bestimmte URL aufgerufen wird. Beispiel `routing.py`: 
```python
from django.conf.urls import url

from . import consumers

websocket_urlpatterns = [
    url(r'^ws/chat/(?P<room_name>[^/]+)/$', consumers.ChatConsumer),
]
```

Wenn die URL `ws/chat/<room_name>` aufgerufen wird, wird die Klasse ChatConsumer aufgerufen.

### Consumer
Consumer sind dafür da um eine Verbindung aufzubauen oder wieder zu verlassen. Ebenso senden diese Nachrichten und nehmen diese entgegen. Ein Beispiel für einen Consumer ist dieses hier.

```python 
# chat/consumers.py
from channels.generic.websocket import WebsocketConsumer
import json

class ChatConsumer(WebsocketConsumer):
    def connect(self):
        self.accept()

    def receive(self, text_data):
        text_data_json = json.loads(text_data)
        message = text_data_json['message']

        self.send(text_data=json.dumps({
            'message': message
        }))
```

Dieser nimmt eine Verbindung entgegen und empfängt Nachrichten. Diese Nachrichten werden dann zurück zum Sender geschickt, wodurch der User die Nachrichten in seiner Textbox sieht(Dafür muss dann ein entsprechendes Template vorhanden sein).

### Channel Layer
Ein Channel Layer ist dafür da, dass die Consumer miteinander kommunizieren können. Jeder Consumer hat automatisch einen Channel Namen, mit diesem Namen kann der Consumer mit dem Channel Layer kommunizieren. Damit Channels mit einander kommunizieren können, müssen sie entweder den Namen eines anderen Channels kennen, oder in einer Gruppe von Channel sein.