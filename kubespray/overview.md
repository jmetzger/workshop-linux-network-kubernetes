# Kubespray 

  * Also auf Basis von ansible und kubeadm 

## Single-Node 

```
[all]
node1 ansible_host=192.168.1.10

[kube_control_plane]
node1

[kube_node]
node1

[etcd]
node1
```

### Cluster erweitern 

```
[all]
node1 ansible_host=192.168.1.10
node2 ansible_host=192.168.1.11  # neu
node3 ansible_host=192.168.1.12  # neu

[kube_control_plane]
node1

[kube_node]
node1
node2  # neu
node3  # neu

[etcd]
node1
```

```
ansible-playbook -i inventory/mycluster/inventory.ini \
  scale.yml \
  --limit=node2,node3
```

