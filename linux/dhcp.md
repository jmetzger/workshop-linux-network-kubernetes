# DHCP 

Hier ist eine **klare, trainingsgeeignete Erklärung** für **DHCP Discover**, so wie du sie in einem Unterricht oder IT-Training einsetzen kannst – anschaulich, technisch korrekt und mit einem kleinen Ablaufdiagramm.

---

## 🧩 **Wie funktioniert DHCP Discover?**

(DHCP Schritt 1 im DORA-Prozess)

DHCP (Dynamic Host Configuration Protocol) sorgt dafür, dass ein Gerät automatisch eine **IP-Adresse**, **Gateway**, **DNS** usw. bekommt.

Der Prozess besteht aus 4 Schritten:
**D**iscover → **O**ffer → **R**equest → **A**ck

Wir konzentrieren uns auf den ersten Schritt: **Discover**.

---

## 🚀 **Was macht „DHCP Discover“?**

Wenn ein Gerät (z. B. Laptop) neu ins Netzwerk kommt, **kennt es seine IP noch nicht**.
Also sendet es eine Nachricht ins Netzwerk:

👉 *„Hallo, gibt es hier einen DHCP-Server, der mir eine IP geben kann?“*

Diese Nachricht ist der **DHCP Discover**.

---

## 📢 **Wichtig: Es ist ein Layer-2-Broadcast**

**DHCP Discover wird an alle Geräte im LAN ausgesendet**, weil der Client
noch nicht weiß, wer der DHCP-Server ist.

### 🔹 Ziel-MAC-Adresse:

```
ff:ff:ff:ff:ff:ff
```

→ **Broadcast an alle**

### 🔹 Quell-MAC-Adresse:

MAC-Adresse des Clients (z. B. seines LAN-Ports)

### 🔹 IP-Schicht:

* Source IP: `0.0.0.0` (Client hat noch keine IP)
* Destination IP: `255.255.255.255` (Broadcast)

---

## 🧠 **Warum Broadcast?**

Weil der Client **nicht** weiß:

* Welche IP der Server hat
* Ob es überhaupt einen Server gibt
* Wo sich im Netz ein Server befindet

**Broadcast → Alle hören zu → DHCP-Server antwortet.**

---

## 🛜 **Was macht der Switch?**

Ein Switch leitet Broadcasts **an alle Ports im gleichen Layer-2-Segment** weiter.
(Das ist die sogenannte **Broadcast-Domain**.)

Daher erreichen DHCP-Discover-Pakete alle Geräte im LAN.

---

## 📦 **DHCP Discover – Ablauf (vereinfacht)**

---

### **1️⃣ Client startet**

* Kein IP → benutzt `0.0.0.0`
* Kennt den Server nicht

### **2️⃣ Client sendet: DHCPDISCOVER**

* MAC → ff:ff:ff:ff:ff:ff (Broadcast)
* IP → 255.255.255.255 (Broadcast)
* UDP Port 68 → 67

### **3️⃣ Alle Geräte im LAN empfangen es**

* Switch broadcastet an alle Ports
* Nur der DHCP-Server reagiert

### **4️⃣ Server antwortet mit DHCPOFFER**

Und dann kommen Schritt 2–4 (Offer → Request → Ack).

---

## 🧪 **Mini-Übung (ideal fürs Training)**

### **0. Vorbereitung**

```bash
sudo apt update

# ARP / Netzwerk / DHCP / Sniffer Tools
sudo apt install -y \
  isc-dhcp-client \
  tcpdump
```

### **1. DHCP Traffic beobachten**

```bash
sudo tcpdump -i eth0 -n port 67 or port 68
```

Währenddessen:

### **2. DHCP erneuern**

```bash
sudo dhclient -r
sudo dhclient
```

Achte auf:

* DHCPDISCOVER
* DHCPOFFER
* DHCPREQUEST
* DHCPACK

---

## 🧑‍🏫 Perfekte Kurz-Erklärung

> **DHCP Discover ist die erste Nachricht eines Geräts ohne IP, um einen DHCP-Server zu finden.
> Es wird als Broadcast an alle Geräte im LAN gesendet, weil der Client die IP des Servers noch nicht kennt.
> DHCP arbeitet deshalb zu Beginn komplett mit Broadcasts, bis eine IP vergeben wurde.**

---

<img width="940" height="500" alt="image" src="https://github.com/user-attachments/assets/477bea19-408c-487b-8368-31ee66b6ed9e" />


