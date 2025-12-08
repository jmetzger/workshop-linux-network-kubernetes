# Firewall für North/South (Kubernetes Cluster) on-premise 

## Bevorzugt: Firewall vor Kubernetes 

  * Wenn ich das Glück habe, dass eine Firewall vor dem Kubernetes Cluster zur Verfügung steht, ist der beste Weg diese zu verwenden
  * Diese ist immer der Fall bei den Cloud Providern und von Fall-zu-Fall auch on-prem beim Kunden

## Keine weitere Firwewall davor ? (Wie gehen wir vor):

  * Neben dem Workload im Kubernetes Cluster wollen wir auch den eigehenden Traffic auf Kubernetes-fremden Ports beschränken (bspw. Firewall)
  * Direkt mit iptables oder firewalld zu arbeiten sollten wir vermeiden, um nicht mit Kubernetes ins Gehege zu kommen.
  * Das Mittel der Wahl sind

## Hintergründe Firewall (Network Policies u.a.) des CNI-Providers 

   * Jeder CNI - Provider ist zwar verpflichtet die NetworkPolicy von Kubernetes zu unterstützen, bringt aber auch seine eigenen Ressourcen (CRD's mit)
   * Hier gilt es nachzuschauen, ob es etwas auf der Host-Ebene gibt, dass wir verwenden können

## Konkretes Beispiel mit Calico 

  * Achtung: Aktuell ungetestet

```
cd
mkdir -p manifests/firewall-calico
cd manifests/firewall-calico
```

```
nano 01-host.yml
```

```
# Zunächst werden die Eintiegspunkte festgelegt
# Interface wird hier ausserdem festgelegt
# Node verweist auf den Namen des nodes 
apiVersion: projectcalico.org/v3
kind: HostEndpoint
metadata:
  name: node-1-eth0
  labels:
    role: worker
spec:
  node: worker-1
  interfaceName: eth0  # Physisches Interface
```

```
nano 02-policy.yml
```

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: block-ssh
spec:
  selector: role == 'worker'  # Matcht HostEndpoint Labels
  ingress:
  - action: Deny
    protocol: TCP
    destination:
      ports: [22]
```

```
kubectl apply -f .
```
