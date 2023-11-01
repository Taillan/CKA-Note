# CKA-Note

Documentation :
kubeadm init 


# kubectl-controller-manager
Il host tout les controller
Dans ses options on peut sélectionné les controller a installé. Par défaut ils sont tous installé

Kubeadm doesnot isntall kueblet automatiquement

```bash
ps -aux | COMPOSANT (kubelet kubes-scheduler etc)
```

# Taint
Met une whitelist sur les nodes (seul les pod avec la bonne taint peuvent se déployer)
```bash
kubectl taint node nodes-name key=value:taint-effect #(Noschdeule | PreferNoSchedule | NoExecute )
```

# DaemonSet
Deploy a pod on each node ( nodeProxy for example)

# Static PODs
Mettre dans ce dossier `/etc/kubernetes/manifests` des .yaml
Il check periodiquement pour crer ces pods 