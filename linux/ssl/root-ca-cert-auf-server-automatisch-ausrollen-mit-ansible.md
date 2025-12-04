# Komplettes Ansible CA-Deployment

## 0. Installation (Vorbereitung) - Ubuntu 

```
apt update
apt install ansible -y
```

## 1. Verzeichnisstruktur erstellen

```bash
mkdir -p ansible-ca-deployment/{inventory,playbooks,files}
cd ansible-ca-deployment
```

## 2. Dateien erstellen

**ansible.cfg:**
```ini
[defaults]
inventory = inventory/hosts
host_key_checking = False

[privilege_escalation]
become = True
become_method = sudo
```

**inventory/hosts:**
```ini
[webservers]
web-ubuntu01.firma.local ansible_host=192.168.1.10
web-ubuntu02.firma.local ansible_host=192.168.1.11

[databases]
db-centos01.firma.local ansible_host=192.168.1.20
db-rocky01.firma.local ansible_host=192.168.1.21

[all:vars]
ansible_user=admin
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

**playbooks/deploy-ca.yml:**
```yaml
---
- name: Deploy CA certificate to all servers
  hosts: all
  become: yes
  
  vars:
    ca_paths:
      Debian: /usr/local/share/ca-certificates/
      RedHat: /etc/pki/ca-trust/source/anchors/
    ca_commands:
      Debian: update-ca-certificates
      RedHat: update-ca-trust
  
  tasks:
    - name: Detect OS family
      debug:
        msg: "OS Family: {{ ansible_os_family }}"
    
    - name: Copy CA certificate
      copy:
        src: firma-ca.crt
        dest: "{{ ca_paths[ansible_os_family] }}firma-ca.crt"
        owner: root
        group: root
        mode: '0644'
    
    - name: Update CA trust store
      command: "{{ ca_commands[ansible_os_family] }}"
      register: ca_update
    
    - name: Show update result
      debug:
        msg: "{{ ca_update.stdout_lines }}"
```

**files/firma-ca.crt:**
```bash
# CA-Zertifikat hierhin kopieren
cp /pfad/zu/deiner/ca.crt files/firma-ca.crt
```

## 3. Verzeichnisstruktur prüfen

```bash
tree
```

```
ansible-ca-deployment/
├── ansible.cfg
├── inventory/
│   └── hosts
├── files/
│   └── firma-ca.crt
└── playbooks/
    └── deploy-ca.yml
```

## 4. Ausführen

**Test-Modus (Dry-Run):**
```bash
ansible-playbook playbooks/deploy-ca.yml --check
```

**Verbindung testen:**
```bash
ansible all -m ping
```

**CA verteilen:**
```bash
# Alle Server
ansible-playbook playbooks/deploy-ca.yml

# Mit Ausgabe
ansible-playbook playbooks/deploy-ca.yml -v
```

## 5. Verifizieren

```bash
# Trust Store auf Server prüfen
ansible all -m shell -a "ls -la /usr/local/share/ca-certificates/ || ls -la /etc/pki/ca-trust/source/anchors/"

# Zertifikat testen
ansible all -m shell -a "curl https://internal-server.firma.local"
```

## Ausgabe-Beispiel

```
PLAY [Deploy CA certificate to all servers] **************************

TASK [Detect OS family] ***********************************************
ok: [web-ubuntu01.firma.local] => {
    "msg": "OS Family: Debian"
}
ok: [db-centos01.firma.local] => {
    "msg": "OS Family: RedHat"
}

TASK [Copy CA certificate] ********************************************
changed: [web-ubuntu01.firma.local]
changed: [db-centos01.firma.local]

TASK [Update CA trust store] ******************************************
changed: [web-ubuntu01.firma.local]
changed: [db-centos01.firma.local]

PLAY RECAP ************************************************************
web-ubuntu01.firma.local   : ok=4    changed=2
db-centos01.firma.local    : ok=4    changed=2
```

