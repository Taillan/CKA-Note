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
netstat -anp | grep etcd # Compte les connection sur etcd
```
ETCD Port Number explicaiton
Correct! That's because 2379 is the port of ETCD to which all control plane components connect to. 2380 is only for etcd peer-to-peer connectivity. When you have multiple controlplane nodes. In this case we don't.

# A revoir 
- Target Port in svc
- openssl x509 -in  /etc/kubernetes/pki/ca.crt -text -noout

## Process 
1. ssh to the node
2. systemctl status containerd
3. systemctl status kubelet

Config de kubelet : /var/lib/kubelet/config.yaml
Attention aux [port](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)

```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUt2Um1tQ0h2ZjBrTHNldlF3aWVKSzcrVVdRck04ZGtkdzkyYUJTdG1uUVNhMGFPCjV3c3cwbVZyNkNjcEJFRmVreHk5NUVydkgyTHhqQTNiSHVsTVVub2ZkUU9rbjYra1NNY2o3TzdWYlBld2k2OEIKa3JoM2prRFNuZGFvV1NPWXBKOFg1WUZ5c2ZvNUpxby82YU92czFGcEc3bm5SMG1JYWpySTlNVVFEdTVncGw4bgpjakY0TG4vQ3NEb3o3QXNadEgwcVpwc0dXYVpURTBKOWNrQmswZWhiV2tMeDJUK3pEYzlmaDVIMjZsSE4zbHM4CktiSlRuSnY3WDFsNndCeTN5WUFUSXRNclpUR28wZ2c1QS9uREZ4SXdHcXNlMTdLZDRaa1k3RDJIZ3R4UytkMEMKMTNBeHNVdzQyWVZ6ZzhkYXJzVGRMZzcxQ2NaanRxdS9YSmlyQmxVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ1VKTnNMelBKczB2czlGTTVpUzJ0akMyaVYvdXptcmwxTGNUTStsbXpSODNsS09uL0NoMTZlClNLNHplRlFtbGF0c0hCOGZBU2ZhQnRaOUJ2UnVlMUZnbHk1b2VuTk5LaW9FMnc3TUx1a0oyODBWRWFxUjN2SSsKNzRiNnduNkhYclJsYVhaM25VMTFQVTlsT3RBSGxQeDNYVWpCVk5QaGhlUlBmR3p3TTRselZuQW5mNm96bEtxSgpvT3RORStlZ2FYWDdvc3BvZmdWZWVqc25Yd0RjZ05pSFFTbDgzSkljUCtjOVBHMDJtNyt0NmpJU3VoRllTVjZtCmlqblNucHBKZWhFUGxPMkFNcmJzU0VpaFB1N294Wm9iZDFtdWF4bWtVa0NoSzZLeGV0RjVEdWhRMi80NEMvSDIKOWk1bnpMMlRST3RndGRJZjAveUF5N05COHlOY3FPR0QKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  usages:
  - digital signature
  - key encipherment
  - client auth
   ```
```bash
$ kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development

$ kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```
```bash
$ kubectl auth can-i update pods --as=john --namespace=development
```
