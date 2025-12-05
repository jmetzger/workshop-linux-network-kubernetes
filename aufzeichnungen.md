# Aufzeichnungen 

```
#  Workshop: LInux, Network und Kubernetes 

## Verteiler 
gerne auch email an:
    Jochens Mail 
j.metzger@t3company.de

## Jochen bewerten ;o) 

https://g.page/r/CZcuN2PgwThxEAE/review


### inventory

[all]node1 ansible_host=192.168.1.10[kube_control_plane]node1 [kube_node]node1 [etcd]node1



## Documentation 


https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/README.md

## info oAuth
https://istio.io/latest/docs/tasks/security/authorization/authz-custom/



## Übung 5.10 PersitentVolume einbinden 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubernetes-csi/nfs-exercise.md#step-3-persistent-volume-claim



## Info 5.9. Liste der treiber (CSI) 

https://kubernetes-csi.github.io/docs/drivers.html


## Übung 5.8. storageclasses 

kubectl get storageclasses 
kubectl -n kube-system get ds csi-do-node


## Übung 5.7 csi - überblick 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubernetes-csi/overview.md


## Übung 5.6. sealed-secret erstellen 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/08-sealed-secret.md


## Info 5.5. kubeseal / sealed - secret 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/cdeccecc5b3d03c94509a8ed3ad0b13867e08092/kubernetes/secrets/sealed-secrets.md

## Info 5.4. secret 

kubectl create secret generic mariadb-secret --from-literal=MARIADB_ROOT_PASSWORD=11abc432 --dry-run=client -o yaml > 01-secrets.yml

cat 01-secrets.yaml
kubectl apply -f  . 
kubectl describe secrets mariadb-secret 
kubectl get secrets mariadb-secret -o yaml 

# schritt 1.
# im deployment 
nano 02-deploy.yml 
# ändern configMapRef -> auf secretRef 
# name ändern  von mariadb-configmap -> mariadb-secret 
kubectl apply -f . 

kubectl exec deployment/mariadb-deployment -- env




## Info 5.3 configmap 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/06a-configmap-mariadb.md
Schritt 1 + Schritt 2 


## Info 5.2 compability 

https://github.com/kubernetes-sigs/gateway-api/tree/main/conformance/reports
von Michael: - Major player for Gateway API: https://konghq.com/company/why-kong


## Info 5.1 session stickyness 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/ingress/traefik/session-stickyness.md


## Übung 4.12 helm chart erstellen q

cd 
mkdir helm-charts
helm create voellig-egal
helm install nichtsoegal voellig-egal
helm list 
helm uninstall nichtsoegal



## Übung 4.11 traefik mit letsencrypt und cert-manager 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/ingress/https-letsencrypt-ingress-traefik.md


## Übung 4.11

kubectl delete ns jochen 
kubectl create ns jochen 

## Übung 4.10 

kubectl -n jochen get secret example-tls 
kubectl -n jochen get secret example-tls -o yaml 

## Übung 4.9. 
https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/04-ingress-traefik-with-hostnames-deployment.md#ingress-nginx-cd 
## Info 4.7.5 artifacthub.io
 


## Übung 4.7. Anatomie einer Webanwendungen

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/anatomie-einer-webanwendung.md


## Übung 4.6.: Umstellung auf LoadBalancer

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/03b-service.md#example-iii-service-mit-loadbalancer-externalip

## Übung 4.5. Umstellung auf NodePort 
 https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/03b-service.md#example-ii--short-version


## Übung 4.4. DNS - Resolution

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubernetes-networks/dns-resolution-services.md

## Übung 4.3. Netzwerkverbindung zu pod testen 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/tipps-tricks/verbindung-zu-pod-testen.md

kubectl get pods -l web=my-nginx -o wide
kubectl get svc svc-nginx

kubectl run podtest --rm -it --image busybox 
ping -c4  <einer-der-pods-ip>
wget -O - http://<einen-der-pods>
# service anpingen geht nicht 
ping -c4   10.109.24.40


  image: traefik/whoami


## Übung 4.2 Service 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/03b-service.md



## Info 4.1.5 zu Ansible

https://github.com/jmetzger/workshop-linux-network-kubernetes/commit/aab9528b3f2be7e6efc9f5f95fd246485cfda249





## Übung 4.1 ca-cert auf allen maschine ausrollen mit ans. 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/ssl/root-ca-cert-auf-server-automatisch-ausrollen-mit-ansible.md


## Übung 3.14 Deployment 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/03-nginx-deployment.md

## Übung 3.13 replicaset 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/01a-replicaset-nginx.md

## Übung 3.12 testpod löschen

kubectl delete po testpod 

## Übung 3.11 pod erstellen 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl-examples/01-pod-nginx.md

## Info 3.10 Bauen einer Applikation mit ResourceObjekten

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/bauen-einer-webanwendung.md

## Übung 3.9. pod 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl/run-with-example.md
kubectl get nodes 

# Übung 3.8. einrichten mit kubectl und namespace

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubectl/kubectl-einrichten.md


## Übung 3.7. 

kubectl cluster-info 


## Übung 3.6. ssh - server verbinden 

IP: 209.38.231.229
tln1 - jochen 
tln2 - hannes
tln3 - Stefan
tln4 - Frank S
tln5 - Uwe
tln6 - frank h
tln7 Alex
tln8 - Max
tln9Arthur
tln10 - Burkhard
tln11Michael
tln12 kuPeter
tln13 CHS


# Info 3.5. Architektur

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/kubernetes/architecture.md


## Info 3.4 timers 

systemctl list-units -t timer 
 
systemctl list-timers 
systemctl cat certbot.service 

journalctl -u certbot.timer 
journalctl -u certbot.service 

## Übung 3.3.  apache installieren, ssl aktivieren und virtualhost einrichten 





https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/ssl/letsencrypt-apache2-ubuntu.md


## Übung 3.2 Einloggen  über ssh und in den root-benutzer wechseln 





## Übung 3.1

user: 11trainingdo 

app.tln1.t3isp.de  164.90.172.37  jochen 
app.tln2.t3isp.de 165.22.69.94 hannes 
app.tln3.t3isp.de 165.232.70.4 stefan
app.tln4.t3isp.de 164.92.174.165 frank s 
app.tln5.t3isp.de 138.68.66.109  Uwe
app.tln6.t3isp.de 138.68.95.69 chs
app.tln7.t3isp.de 165.22.93.200 Alex
app.tln8.t3isp.de 64.225.106.47Max
app.tln9.t3isp.de 142.93.163.19 Arthur
app.tln10.t3isp.de 178.128.196.120 Burkhard
app.tln11.t3isp.de 165.232.113.70Michael
app.tln12.t3isp.de 207.154.224.67 Peter
app.tln13.t3isp.de 207.154.238.61frank h






## Übung 2.6 do-release-upgrade 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/upgrade/ubuntu-22-auf-24.md
do-release-upgrade -f DistUpgradeViewNonInteractive

firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p tcp --dport 443 -j l-cmd --runtime-to-permanent 




## Übung 2.5 DHCP-Server aufsetzen 

lhttps://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/dhcp-server-aufsetzen.md

## Übung 2.4 DNS  - bind9 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/install-bind9-ubuntu.md


## Übung 2.3 dig 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/dig.md#%EF%B8%8F-3-praktische-abfragen-mit-dig

## Übung 2.2 arp 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/arp.md



## Übung 2.1 firewall ingress / egress 

https://github.com/jmetzger/workshop-linux-network-kubernetes/blob/main/linux/firewall.md

firewall-cmd --zone=public --change-interface=enp0s8


# enp0s8 in public 
firewall-cmd --get-active-zones 
# auch den http dienst hier mit aufgelistet haben
firewall-cmd --list-all 



## Übung 1.12 schärfere Regeln  / aufgeschoben 

man firewalld.zones 
firewall-cmd  --zone=drop --change-interface=enp0s8 

pin

## Übuing 1.11 firewalld installieren 

systemctl status firewalld 
apt search ^firewalld 
apt install firewalld -y 
systemctl status firewalld 

firewall-cmd --state 
# Was ist Betrieb für die Zone 
firewall-cmd --list-all 
firewall-cmd --get-active-zones 

# Interfacd zu der Zone hinzufügen 
firewall-cmd --zone=public --add-interface=enp0s8
# jetzt in der public zone 
firewall-cmd --get-active-zones 

firewall-cmd --runtime-to-permanent 

firewall-cmd --list-all 
# Alle Services 
firewall-cmd --get-services 
# 
firewall-cmd --info-service=http 

## service http freischalten 
firewall-cmd --add-service=http 

firewall-cmd --runtime-to-permanent 








## Übung 1.10 ufw deinstallieren 

ufw status  
apt remove ufw 
apt search ^firewalld
systemctl stop ufw 


# Info 1.9. 
unattended upgrades 

cd /etc/apt/apt.conf.d 
less  50unattended-upgrades




## Übung 

## Übung 1.8 nmap 

apt search ^nmap 
apt install nmap -y 
nmap -A 192.168.56.101 

## Übung 1.7 konnektivität (telnet) 

telnet 192.168.56.101 80


## Übung 1.6 Konnektivität testen (curl)

apt install curl 
apt autoremove -y 
curl http://192.168.56.101 
# nur Header 
curl -I http://192.168.56.101

dpkg -l | grep curl 
apt list --installed | grep curl 



## Übung 1.5 openssh-server installieren

apt install openssh-server -y 
systemctl start ssh
systemctl enable ssh 

## Übung 1.4 Paket installieren 

apt update 
# Sucht nach Vorkommen am Anfang der Zeile 
apt search ^apache2
apt install apache2 -y 

systemctl status apache2.service 
lsof -i

## Übung 1.3 Verbindung per ssh 



## Übung 1.2 läuft ssh 

systemctl statusd 
sudo su -
ip a 

 - -
## Übung Installieren von Ubuntu 22.04 


## Zeiten 

09:00 - 10:30 Block I 
10:30 - 10:45 Frühstückspause 
10:45 - 12:00 Block II 
12:00 - 13:00 Mittag 
13:00 - 14:30 Block III
14:30 - 14:45 Teatime 
14:45 - 16:30 Block IV


## Agenda 

1. Netzwerkgrundlagen (Was ist wichtig ?) 
Komponenten wie Router, Switches sowie IP-bezogene Themen (MAC-Adressen, ARP, NAT)

Überblick. Tools, welche gibt es und wie verwende. 

Netzwerkdienste wie DNS und DHCP

DNS-Server aufsetzen. 
(welchen Daten auflösbar) 
Praxis mit kennenlernen dig 

Sicherheit in der Netzwerkkommunikation: Zertifikate, Firewalls, Proxy-Systeme
2. Linux Essentials

Linux zu DNS-Server 
Debian / Ubuntu-Systeme. 



Grundkonzepte und Installation von Linux-Systemen

Ubuntu 24.04 LTS mit ServerInstallation 
(alles auf Kommando-Line) 

nano

ServerInstallation und arbeiten dann aber auf der Konmandozeile  


Systemkonfiguration und Administration über Kommandozeilen-Tools
Zertifikate vom Kunden verwenden können 
(ACME) 

fqdn. Auf das muss ein Zertifikat erstellt werden .
(Web) 


Optional: (Einführung in grundlegendes Shell-Scripting)
Firewalling 
3. Kubernetes Essentials

Traeffik wie geht da ein Zertfikat. 

Ranger 

Architekturüberblick und zentrale Komponenten (API Server, Controller Manager, Scheduler)
Einführung in Pods, Workloads, Storage, ConfigMaps und Secrets
Netzwerk und Konnektivität über Ingress (Traeffik) 
Lifecycle-Management: Updates, Upgrades, Rollbacks, Helm-Charts
Monitoring und grundlegende Fehleranalyse
Trainer Überblick:  (Network) - Kubernetes. 


## Linux Fragen 

## Fragen: 
    
Klassiche umgesetzt: 

In dotnet auf dotnet 8 (geht das auch unter Linux)


Notizen vom 10.11.2025  (Was ist noch wichtig) 

Gibt es automatisierung / best practices  Zertfikate 
(Es wird ausser ACME da noch….. ?) 



## Kubernetes - Fragen

Kubernetes

Nodes absichern 
Was kann da machen zum Absichern (SSL-Verschlüsselung)
mtls -> Service Mesh / Netzwerkkomponenten 

10-15 % Overhead - Trainer 


Storage bereitstellen (wie geht es)
z.B. s3 - kompatiblen 

Sehr vielen Bildern zu tun…. 1 GB Größe für CT. 
(Massendaten 

Datenbank (Befunddaten) 
Datenbanken um 


Wie behandle ich die Konfigurationsdaten 
(Was ist mit systemweite Daten) .

Tooling: config.net 
(Wie setzten wir das um ?) 


Secret-Handler 
Mit Erstinbetriebnahme start noch kein Cluster, aber onboarding 
azure. 



```
