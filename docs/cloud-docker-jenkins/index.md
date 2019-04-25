# Cloud Computing and CI

## Continuous Integration Tools

Es folgt eine Liste von CI-Tools mit einer kurzen Beschreibung und einigen Vor- bzw. Nachteilen. Es wurden nur frei nutzbare Tools in die Liste aufgenommen, da es ein schier grenzenlose Zahl von kommerziellen CI-Versionen gibt, die sich nur wenig unterscheiden oder spezielle Buildsysteme bereitstellen.

**Jenkins**

Jenkins ist bei nahezu jeder CI-Tool-Liste die Nummer eins. Es ist frei zugänglich, sprich Open-Source und bietet durch seine Plugin-Infrastruktur die Möglichkeit seine Konfiguration an viele verschiedene Projekte (und Sprachen) anzupassen. Jenkins ist eine Java Applikation und hat auch seine Wurzeln bei der Firma "Sun Microsystems" die 2010 von Oracle aufgekauft wurde.

- Vorteile
  - Sehr flexible an verschiedene Projektformen anpassbar (Plugins)
- Nachteile
  - Hoher Konfigurationsaufwand und Eigenleistung notwendig
- Abhängigkeiten
  - Java Runtime Env.

**Travis CI / Circle CI**

Die beiden CI-Tools stellen ein Cloud-basierte Lösung eines Build und Deploysystem dar. Travis sowie Circle bieten die Nutzung für Open-Source Projekte umsonst an die auf Github gehostet werden. Aber auch diese sind auf Build-Anzahl etc. beschränkt. Diese Tools nehmen einem sehr viel Denkarbeit ab und man kann sich aufs Wesentliche konzentrieren.

- Vorteile
  - Nimmt einem sehr viel Arbeit ab, schnell konfigurierbar
  - Bietet Support für fast alle gängigen Programmiersprachen und Projekte
  - Kann auch kommerziell selbst gehostet werden 
- Nachteile
  - Bei Free-Version auf Github beschränkt
- Abhängigkeiten
  - X

**Gitlab CI Community**

Gitlab CI stellt auch eine Cloud-basierte Lösung dar, bei der allerdings die Self-Hosted-Core Version gratis ist. Entwickelt wird Gitlab in Ruby und Go.

- Vorteile
  - Self-Hosted Git Server mit Issue Tracker, CI, CD ....
  - Integrierter GitHub-Ersatz 
- Nachteile
  - Höherer Konfigurationsaufwand bei Self-Hosted
- Abhängigkeiten
  - X

## Jenkins

**Möglichkeiten Jenkins aufzusetzen**

- Man kann Jenkins auf allen Systemen installieren, dafür benötigt man nur ein OS mit einem installierten Java (7 oder 8, mit 9 läuft es wohl noch nicht). Möchte man die Builds mit Docker-Containern ausführen, benötigt man natürlich auch noch ein lauffähiges Docker.

**Jenkins testweise aufsetzten** (2.1.6.X)

- Ich habe Jenkins auf einem Raspberry 2B installiert, den ich noch zuhause hatte. Die Performance lässt natürlich zu wünschen übrig, aber so hat man gleich das feeling eines Cloud-Servers. Konfiguriert wird Jenkins über das bereitgestellte Webinterface, Konfigurationen am Server selbst führe ich durch eine SSH-Verbindung durch.

- ```bash
  # Java installieren
  $ sudo apt-get install oracle-java8-installer
  # Docker installieren
  $ curl -fsSL https://get.docker.com -o get-docker.sh
  $ sudo sh get-docker.sh
  $ sudo usermod -aG docker your-user
  # Jenkins installieren
  $ wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
  # deb https://pkg.jenkins.io/debian-stable binary/ >> /etc/apt/source.list
  $ sudo apt-get update
  $ sudo apt-get install jenkins
  ```

- Pipeline anlegen
  
  - 
  
- Jenkins Plugins?

- Was sind Pipelines?

- Wie funktioniert ein `Jenkinsfile`?

- Dependencies?

## Docker / Cloud Computing

- Jenkins in einem Docker Container?
- Scaling Jenkins mit Docker und Kubernates?
- Scaling Jenkins mit Docker Compose?
- Docker-Swarm?
- Eigene Docker Container erstellen?

## Quellen

**Continuous Integration Tools**

- [50 CI Tools](https://stackify.com/top-continuous-integration-tools/)
- [Jenkins in Docker und mit Docker und für Docker](https://www.oose.de/blogpost/jenkins-in-docker-und-mit-docker-und-fuer-docker/)
- [CircleCI vs Travis CI vs Jenkins](https://hackernoon.com/continuous-integration-circleci-vs-travis-ci-vs-jenkins-41a1c2bd95f5)
- Wikipedia

**Jenkins**

- [Dokumentation](https://jenkins.io/doc/)