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
# das macht keinen eintrag im Cache
# nur zum Testen
arping -I <interface> <IP>
# Das jedoch schon (eintrag erfolgt)
wget <IP-Adresse>
# z.B.
wget 192.168.56.102
ip neigh 
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

