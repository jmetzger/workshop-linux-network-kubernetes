# Layer2- Netzwerk und B

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
