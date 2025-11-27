# Netzwerk -> Switches 

## Hintergrund 

  * Switches sind auf Layer 2 (Data-Layer) und merken sich nur MAC-Adressen
  * Switches haben einen Mac-Adressen-Tabelle (Sie lernen dynamisch an welchem Port welche MAC-Adresse angeschlossen, erreichbar ist)


```
In diesem Szenario spielen die Switches eine wichtige Rolle bei der Weiterleitung der Pakete zwischen den Geräten, auch wenn sie an verschiedenen Switches angeschlossen sind. Hier ist der Ablauf:

1. Angenommen, Gerät A (am Switch 1) möchte Gerät B (am Switch 2) anpingen. Gerät A sendet zunächst eine ARP-Anfrage als Broadcast an alle Geräte im lokalen Netzwerk.

2. Switch 1 empfängt die ARP-Anfrage und leitet sie an alle seine Ports weiter, einschließlich des Ports, der mit Switch 2 verbunden ist.

3. Switch 2 empfängt die ARP-Anfrage über den Uplink-Port von Switch 1 und leitet sie an alle seine Ports weiter, an denen Geräte angeschlossen sind.

4. Gerät B empfängt die ARP-Anfrage und erkennt, dass es die gesuchte IP-Adresse hat. Es sendet eine ARP-Antwort zurück, die seine MAC-Adresse enthält.

5. Switch 2 empfängt die ARP-Antwort und lernt, an welchem Port Gerät B angeschlossen ist. Dann leitet er die Antwort über den Uplink-Port zurück zu Switch 1.

6. Switch 1 empfängt die ARP-Antwort und lernt, an welchem Port Gerät A angeschlossen ist. Dann leitet er die Antwort an Gerät A weiter.

7. Gerät A empfängt die ARP-Antwort und speichert die IP-Adresse und die dazugehörige MAC-Adresse von Gerät B in seinem ARP-Cache.

8. Jetzt kann Gerät A Pakete direkt an die MAC-Adresse von Gerät B senden. Die Switches leiten diese Pakete basierend auf ihren MAC-Adresstabellen weiter, die sie durch die ARP-Anfragen und -Antworten gelernt haben.

Dieser Prozess ermöglicht die Kommunikation zwischen Geräten, auch wenn sie an verschiedenen Switches angeschlossen sind, solange die Switches miteinander verbunden sind. Die Switches lernen dynamisch, an welchen Ports die Geräte angeschlossen sind, indem sie den ARP-Verkehr beobachten.

```
