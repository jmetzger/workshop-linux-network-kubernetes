# Firewall für North/South (Kubernetes Cluster) on-premise 

## **Wichtiger Hinweis**

**Bitte verwendet nicht direkt *firewalld* oder *iptables*.**

---

## **Szenario 1: Cloud**

Hier nutzt ihr **immer die vorgeschaltete Firewall des Cloud-Providers**.

---

## **Szenario 2: On-Prem ohne vorgeschaltete Firewall**

*(Ich muss dazu sagen, dass ich keine Kosten und Mühen gescheut habe …
Spaß 😉 … letzteres stimmt tatsächlich.)*

Ich habe ein Cluster mit **kubeadm** und **Calico** hochgefahren – und was soll ich sagen, es hat eben nicht sofort funktioniert.

---

## **Grundsatz**

**Verwendet immer die Firewall des CNI-Providers.**

### Was bedeutet das?

Wie ihr wisst, muss das Netzwerk in Kubernetes irgendwie gebaut werden.
Kubernetes macht das **nicht selbst**, sondern lässt es durch ein CNI-Plugin erledigen.
Stichwort: **CNI (Container Network Interface)**.

Der CNI-Provider unterstützt meistens auch **Kubernetes NetworkPolicies**, stellt aber **eigene Ressourcen** bereit, um Firewall-Themen zu managen.

→ **Schaut also immer nach, wie euer CNI-Provider das macht und was er anbietet.**

---

## **Beispiel: Calico**

Calico bringt eigene Konzepte für Node-Security mit:

### **HostEndpoints**

* Können **automatisch** erstellt werden.
* Sie repräsentieren die Netzwerk-Interfaces der Nodes.

### **GlobalNetworkPolicies**

* Gelten normalerweise **innerhalb des Clusters**.
* In Kombination mit HostEndpoints wirken sie auch auf **Ports außerhalb des Cluster-Netzwerks** – also direkt auf Node-Level.

---

## **Wichtiges Verhalten der Calico HostEndpoints**

Sobald eine **GlobalNetworkPolicy** existiert, deren Labels auf einen HostEndpoint matchen:

* **Blockiert Calico alles**, was

  * nicht explizit erlaubt ist **UND**
  * nicht in den **Failsafe Rules** steht.

Failsafe-Regeln:
[https://docs.tigera.io/calico/latest/reference/host-endpoints/failsafe](https://docs.tigera.io/calico/latest/reference/host-endpoints/failsafe)

---

## **Default-Verhalten**

Bevor ihr eigene Policies definiert, lassen die HostEndpoints alles durch,
denn das Profil:

```
projectcalico-default-allow
```

ist aktiv.

---

## **Verifikation**

Ich habe das getestet – und **es funktioniert genau so**.
Das war mir wichtig, damit wir es wirklich sicher wissen.

---

## **Weiterführende Anleitung**

Ihr könnt das hier Schritt für Schritt durcharbeiten:

👉 [https://www.tigera.io/blog/securing-kubernetes-nodes-with-calico-automatic-host-endpoints/](https://www.tigera.io/blog/securing-kubernetes-nodes-with-calico-automatic-host-endpoints/)

Ich schreibe das alles auch hier noch einmal zusammen.

---

Wenn du möchtest, kann ich das Markdown auch erweitern, z. B. mit Code-Beispielen, Diagrammen oder Schritt-für-Schritt-Anleitungen.
