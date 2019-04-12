# MQTT

http://www.hivemq.com/demos/websocket-client/

https://www.hivemq.com/mqtt-toolbox/

## Beschreibung

**Message Queuing Telemetry Transport** (kurz MQTT) ist ein einfach aufgebautes Publish-Subscribe-Protokoll zum Nachrichtenaustausch im Netzwerk. Es benötigt sehr wenig Bandbreite und Funktion auf den Clients, bietet aber trotzdem eine hohe Zuverlässigkeit bei der Nachrichtenübermittlung. Nachrichten werden in sog. Topics, die man sich wie eine Ordnerstruktur vorstellen kann, einsortiert.

### Topologie

Das Herzstück von MQTT ist der Broker, der die Topics und alle darin enthaltenen Nachrichten verwaltet und den Zugriff der einzigen Clients auf diese regelt. 

![MQTT Topologie](img/MQTT%20Topologie.png)

https://www.dataweek.co.za/9101a

### Topics

jraddatz/erdgeschoss/wohnzimmer/temperatur

https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/

### Ablauf





![MQTT Beispiel](img/MQTT_protocol_example_without_QoS.svg)

By Simon A. Eugster - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=70622928

#### QoS - Quality of Service

- Bei der der niedrigsten Stufe 0 handelt es sich im Prinzip um eine *fire'n'forget*-Semantik. Es gibt also keine Garantie, dass die Nachricht überhaupt ankommt. 

- Bei QoS-Level 1 ist sichergestellt, dass die Nachricht mindestens einmal in der Topic-Queue landet (*At-least-once*-Semantik). 

- In der höchsten Stufe 2 garantiert der Broker sogar *exactly-once*: die Nachricht wird also genau einmal abgelegt, nicht öfter und nicht weniger

https://www.heise.de/developer/artikel/Kommunikation-ueber-MQTT-3238975.html