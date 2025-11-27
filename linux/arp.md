# 🧪 **Linux-Übung: ARP & ARPing (Ubuntu 24.04)**

## 🔧 1. Vorbereitung: Installation von `arping`

Ubuntu 24.04 hat `arping` nicht standardmäßig installiert.
Installiere es mit:

```bash
sudo apt update
sudo apt install iputils-arping -y
```

Prüfen, ob es funktioniert:

```bash
arping -h
```

---

## 📘 2. ARP-Cache anzeigen

Der ARP-Cache enthält bekannte Zuordnungen von **IP-Adressen zu MAC-Adressen**.

```bash
ip neigh
```

Oder ausführlicher:

```bash
ip neigh show
```

👉 **Aufgabe:**

* Finde die MAC-Adresse deines Gateways (Default-Gateway ermitteln):

```bash
ip route
```

---

## 🧹 3. ARP-Cache löschen (Ubuntu 24.04)

Seit Ubuntu 22.04+ funktioniert `ip -s -s neigh flush all`.

**Gesamten ARP-Cache leeren:**

```bash
# -s -s zeigt: erweiterte Statistiken 
sudo ip -s -s neigh flush all
```

Erneut prüfen:

```bash
ip neigh
```

👉 **Aufgabe:**

* Leere den Cache
* Überprüfe, dass er wirklich leer ist
* Ping später eine Adresse an und beobachte, wie der Cache sich wieder füllt

---

## 📡 4. ARP-Anfragen mit `arping` durchführen

Wähle ein Ziel im Netzwerk (z. B. das Gateway).

### 🔍 ARP-Request senden:

```bash
arping -I <interface> <IP>
```

```
# Hin -> Broadcast -> Wer hat die IP (bitte MAC-Addresse) 
arping -I eth1 10.135.0.74
# Zurück Unicast (1:1) -> Ich habe sie
```

<img width="898" height="167" alt="image" src="https://github.com/user-attachments/assets/32aeb45c-6232-4407-9459-d8ee8cee24c6" />

Beispiel mit erkanntem Interface:

1. Interface anzeigen:

   ```bash
   ip addr
   ```

2. `arping` senden (Beispiel, bitte `eth0` und IP ersetzen):

   ```bash
   sudo arping -I eth0 192.168.178.1
   ```

👉 **Aufgabe:**

* Führe `arping` gegen dein Gateway aus
* Notiere, ob du ARP-Replys bekommst
* Teste einen nicht existierenden Host (z. B. 192.168.178.222) und vergleiche

---

## 🔄 5. Beobachten, wie sich der ARP-Cache verändert

### Schritt 1: Cache leeren

```bash
sudo ip -s -s neigh flush all
```

### Schritt 2: `arping` ausführen

```bash
sudo arping -I eth0 192.168.178.1
```

### Schritt 3: Cache prüfen

```bash
ip neigh
```

👉 **Aufgabe:**

* Beobachte, wie nach dem `arping` ein neuer Eintrag erscheint
* Prüfe, wie der Status sich verändert
  (z. B. `REACHABLE`, `STALE`, `DELAY`, `FAILED`)
* Warte 1–2 Minuten und prüfe erneut

---

# 🎯 **Zusammenfassung (für Teilnehmer)**

In dieser Übung hast du gelernt:

* wie ARP funktioniert
* wie man den ARP-Cache unter Ubuntu 24.04 einsehen und löschen kann
* wie `arping` ARP aktiv nutzt
* wie der ARP-Cache sich dynamisch aktualisiert

---

Wenn du willst, Sunshine ☀️, erweitere ich dir diese Übung um eine **Mini-Live-Demo**, **Erklärbilder**, oder eine **Multiple-Choice-Testseite** für den Kurs.
