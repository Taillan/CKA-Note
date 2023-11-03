# CKA-Note

Documentation :
kubeadm init 


## kubectl-controller-manager
Il host tout les controller
Dans ses options on peut sélectionné les controller a installé. Par défaut ils sont tous installé

Kubeadm doesnot isntall kueblet automatiquement

```bash
ps -aux | COMPOSANT (kubelet kubes-scheduler etc)
```

## Taint
Met une whitelist sur les nodes (seul les pod avec la bonne taint peuvent se déployer)
```bash
kubectl taint node nodes-name key=value:taint-effect #(Noschdeule | PreferNoSchedule | NoExecute )
```

## DaemonSet
Deploy a pod on each node ( nodeProxy for example)

## Static PODs
Mettre dans ce dossier `/etc/kubernetes/manifests` des .yaml
Il check periodiquement pour crer ces pods 
kubectl cant delete it

you can put kube admin tool in /etc/kubernetes/manifests

# Node Update

```bash
kube-controller-manager --pod-eviction-timeout=5m0s #timeout par défaut avant qu'une node sois considérer ko
kubectl drain node-1 # Permet de vider la node pour la mettre à jour
kubectl cordon # Mark a node unschedulable
kubect uncordon node-1 # Permet de la rendre re schedulable (drain mark it unschidaalble)
```

kubeadm methode
```bash
apt-get upgrade -y kubeadm=1.12.0-00 #Update kubeadm tools first
kubeadm uprage apply #upgrade le controle plane
kubectl get nodes #=> montres les versions de kubelet
apt-get upgrade -y kubelet=1.12.0-00
systemctl restart kubelet
kubectl get nodes #=> Les versions sont update
```

![](./upgrade%20worker.png)

# Usefull command
```bash
ip route show default #(show default gateway)
netstat -nplt #(regarder les port listen)
```
