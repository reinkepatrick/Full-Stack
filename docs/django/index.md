# Django

## Was ist Django?
Django ist ein Python Framework für Backends von Web Applications.

Django ist im release 2.2 und wird unterstützt von Python 3.5 und folgende.

- Läuft auf Apache Web Server (Produktion). Hat eigenen Webserver für Tests
- MTV (Model Template View) als Kern Architektur.
- Object Relational Mapper
- Python unit test framework
- Unterstützung von bekannten Datenbanken
- Platformübergreifend
  
### Bekannte Firmen

- Instagram
- Spotify
- Dropbox
- Mozilla
- NASA
- Pinterest
- Reddit

### Django Struktur
![MTV](./img/MTV-Diagram.png ':size=525')

## Workflow
Als erstes wird python 3.x.y installiert. Danach noch eine Datenbank (Maria DB) einrichten.

Nachdem dann Django installiert wurde, kann man sein Projekt erstellen.

```
$ django-admin startproject mysite
```

### Apps
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

### Views
Views sind zu für die Trennung zwischen Logik und Frontend zuständig. Jede App besitzt eine View. Die einfachste View ist diese hier.

```python
# apps/testapp/view.py
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the test index.")
```

Wenn von dieser view nun die Methode _index_ aufgerufen wird, sendet die view einen HttpResponse mit der Nachricht zurück. 

### URLs
Damit bestimmte Methoden von Views aufgerufen werden können, müssen URLs konfiguriert werden. In der App selber wird eine _urls.py_ die sieht dann mit dem View Beispiel von vorher so aus. 

```python
# apps/testapp/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```
Die Method path kriegt als ersten Parameter die URL, welche aufgerufen werden soll. Dann die Methode, welche aufgerufen wird und mit dem Parameter name kann der URL einen Namen gegeben werden um darauf später referenzieren zu können.


Nun muss noch die root URL konfiguriert werden. Damit dies weiter zur testapp geleitet wird.

```python
# test_project/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('test/', include('apps.testapp.urls')), # Modul von testapp includen.
    path('admin/', admin.site.urls), # Von Django automatisch generiert
]
```