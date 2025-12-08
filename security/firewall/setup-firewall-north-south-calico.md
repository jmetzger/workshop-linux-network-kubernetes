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

```
# So erstellt ihr sie automatisch, wenn ihr mit kubeadm installiert habt
# Die kubecontrollerconfig wird automatisch angelegt, sobald tigera ausgerollt wurde
kubectl patch kubecontrollersconfiguration default --patch='{"spec": {"controllers": {"node": {"hostEndpoint": {"autoCreate": "Enabled"}}}}}'

# Wenn ich das entsprechend ändere, erstellt calico automatisch hostendpoints die wir für die weiteren Schritte brauchen
kubectl get hep
```

<img width="695" height="185" alt="image" src="https://github.com/user-attachments/assets/7f4c77bd-f78c-4380-bb64-75f7d45d46bf" />

```
# Wichtig:
# Achtet auf das Profil: Es läßt erstmal alles durch
kubectl get hep -o yaml
kubectl get profile projectcalico-default-allow -o yaml 
```

## **Wichtiges Verhalten der Calico HostEndpoints**

Sobald eine **GlobalNetworkPolicy** existiert, deren Labels auf einen HostEndpoint matchen:

* **Blockiert Calico alles**, was

  * nicht explizit erlaubt ist **UND**
  * nicht in den **Failsafe Rules** steht.

Failsafe-Regeln:
[https://docs.tigera.io/calico/latest/reference/host-endpoints/failsafe](https://docs.tigera.io/calico/latest/reference/host-endpoints/failsafe)


### **GlobalNetworkPolicies**

* Gelten normalerweise **innerhalb des Clusters**.
* In Kombination mit HostEndpoints wirken sie auch auf direkt auf Node-Level (also auf Traffik von aussen)
* 
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

