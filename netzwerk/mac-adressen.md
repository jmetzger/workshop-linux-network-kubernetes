# Mac Adressen 

## Allgemein 

  * MAC-Adressen sollten in der Theorie weltweit eindeutig sein
  * In der Praxis sind sie es nicht, weil dort teilweise MAC-Adresse selbständig vergeben werden, z.B. bei Virtualisierung
  * Aber: In einem Layer2-Netzwerk, muss ! die MAC-Adresse eindeutig sein (es darf immer nur ein Rechner mit dieser MAC-Adresse antworten)

## Aufbau von MAC-Adressen 

MAC-Adressen (Media Access Control Adressen) sind **48-Bit-Adressen**, die Netzwerkgeräte auf **Layer 2** eindeutig identifizieren. Sie werden typischerweise vom Hersteller vergeben.

---

### 🧱 **Grundaufbau**

Eine MAC-Adresse besteht aus **6 Byte (48 Bit)** und wird meist hexadezimal geschrieben:

```
AA:BB:CC:DD:EE:FF
```

Jedes Paar (`AA`, `BB`, …) repräsentiert **1 Byte = 8 Bit**.

---

### 🔍 **Zweiteilung der MAC-Adresse**

#### **1️⃣ OUI (Organizationally Unique Identifier) – die ersten 3 Byte**

* Vom **IEEE** vergeben
* Identifiziert den **Hersteller**
* Beispiel: `D4:6A:6A` = Apple, `00:1A:2B` = Cisco (nur Beispiele)

#### **2️⃣ Geräte-spezifischer Teil – die letzten 3 Byte**

* Vom Hersteller selbst vergeben
* Jedes produzierte Gerät/Interface bekommt eine eindeutige Nummer

---

#### 🏷️ **Beispiel**

MAC: `3C:5A:B4:12:34:56`

* **3C:5A:B4** → Hersteller (OUI)
* **12:34:56** → Gerät/Interface

---

##### 🔧 **Besondere Bits in der MAC-Adresse**

Mehrere Bits haben besondere Bedeutung:

###### **1. U/L-Bit (Universal/Local)**

* Bit 1 im ersten Byte (Least Significant Bit der ersten Hex-Zahl "AA")
* **0 = Universell** (vom Hersteller vergeben)
* **1 = Lokal** (z. B. durch VM, Docker, WiFi-Spezialsoftware)

Beispiel lokale MACs:

```
02:00:00:...
06:... 
```

###### **2. I/G-Bit (Individual/Group)**

* Bit 0 im ersten Byte
* **0 = Unicast-Adresse**
* **1 = Multicast-Adresse**

Beispiel Multicast-MAC:

```
01:00:5E:xx:xx:xx   (IPv4 Multicast)
33:33:xx:xx:xx:xx   (IPv6 Multicast)
```

---

##### 🧠 Merksatz

> MAC = **6 Byte**, erste Hälfte Hersteller, zweite Hälfte Gerät.
> Bits im ersten Byte bestimmen *global vs. lokal* und *unicast vs. multicast*.

---

###### 📚 Bonus: Wie viele MAC-Adressen gibt es?

48 Bit = **281.474.976.710.656 mögliche MACs** (~281 Billionen).

---

Wenn du willst, Sunshine, kann ich dir auch eine Tabelle mit Beispielen erstellen oder eine Übung für deinen Unterricht.



## Was ist ein Layer 2 - Netzwerk 

  * Ein Layer-2-Netzwerk ist ein Netzwerksegment, in dem Geräte per MAC-Adresse kommunizieren und Switches die Weiterleitung übernehmen
  * Sie teilen sich die gleiche Broadcast - Domain.

```
PC1 ----- Switch ----- PC2
          |
          +---- PC3
```

## Was ist ein Broadcast - Domain 

```
Eine Broadcast-Domain umfasst alle Geräte, die durch Layer-2 (Switch) miteinander verbunden sind und Broadcasts voneinander empfangen können.
```

## Was gehört in eine Broadcast-Domain?

  * Alle Geräte am gleichen Switch, solange keine VLANs konfiguriert wurden
  * Geräte an mehreren Switches, wenn sie in demselben VLAN liegen
  * Geräte im gleichen Layer-2-Netz ohne Router dazwischen

Ein Router trennt Broadcast-Domains – ein Switch nicht.
