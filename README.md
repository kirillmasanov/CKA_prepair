💁‍♂️Resources for preparation
===========================

CKA Curriculum v1.28
====================
## 25% - Cluster Architecture, Installation & Configuration 
```shell
kubeadm config print init-defaults
```
```shell
kubectl api-resources --namespaced=false
```
```shell
kubectl -n rbac-test --as=system:serviceaccount:rbac-test:rbac-test-sa auth can-i get pods
```
Assessing cluster health. Оn a master node. Three endpoints exist - healthz,livez and readyz to indicate the current status of the API server.
```shell
curl -k https://localhost:6443/livez?verbose
```
Backing up etcd. [ETCD FAQ](https://github.com/kodekloudhub/community-faq/blob/main/docs/etcd-faq.md)
```shell
ETCDCTL_API=3 etcdctl snapshot save snapshot.db --cacert /etc/kubernetes/pki/etcd/server.crt --cert /etc/kubernetes/pki/etcd/ca.crt --key /etc/kubernetes/pki/etcd/ca.key
```
Verify the backup:
```shell
sudo ETCDCTL_API=3 etcdctl --write-out=table snapshot status snapshot.db
+----------+----------+------------+------------+
|   HASH   | REVISION | TOTAL KEYS | TOTAL SIZE |
+----------+----------+------------+------------+
| 2125d542 |   364069 |        770 |  	3.8 MB |
+----------+----------+------------+------------+
```
Perform a restore:
```shell
ETCDCTL_API=3 etcdctl snapshot restore snapshot.db
```
Upgrade:
```shell
apt-cache madison <packagename>  # show available versions of a package
apt-get install --only-upgrade <packagename>  # upgrade only that single package, and only if it is installed. If you wish to install the package if it doesn't exist, or upgrade it if it does, you may leave out --only-upgrade.
```
Print join command:
```shell
kubeadm token create --print-join-command
```
Check certificate expiration:
```shell
kubeadm certs check-expiration
```
```shell
/var/lib/kubelet/config.yaml  # kubelet config
```
## 15% - Workloads & Scheduling
```shell
kubectl rollout undo deployment/abc
kubectl rollout status daemonset/foo
kubectl rollout restart deployment/abc
```
```shell
kubectl set image deployment/nginx ginx=nginx:1.9.1
kubectl set image daemonset abc *=nginx:1.9.1
```
## 20% - Services & Networking
```shell
10-42-2-68.default.pod.cluster.local
my-svc.my-namespace.svc.cluster.local
```
```shell
/etc/cni/net.d/  # CNI config file. By default the kubelet looks into this folder to discover the CNI plugins.
```
## 10% - Storage
Access Modes:
*  `ReadWriteOnce` – The volume can be mounted as read-write by a single node
*  `ReadOnlyMany` – The volume can be mounted read-only by many nodes
*  `ReadWriteMany` – The volume can be mounted as read-write by many nodes
## 30% - Troubleshooting
```shell
crictl pods
systemctl daemon-reload
systemctl status kubelet
```
Container logs: `/var/log/containers`
To determine the cluster domain, inspect the coredns configmap. Below indicating `cluster.local`:
```yaml
> kubectl get cm coredns -n kube-system -o yaml
apiVersion: v1
data:
  Corefile: |
    .:53 {
        errors
        health {
          lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
```
Create a simple Pod to use as a test environment:
```shell
kubectl apply -f https://k8s.io/examples/admin/dns/dnsutils.yaml
kubectl exec -it dnsutils -- nslookup kubernetes.default
```
You can verify if queries are being received by CoreDNS by adding the log plugin to the CoreDNS configuration (aka Corefile).
`kubectl -n kube-system edit configmap coredns`
Then add log in the Corefile section per the example below:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns
  namespace: kube-system
data:
  Corefile: |
    .:53 {
        log
        errors
        health
        kubernetes cluster.local in-addr.arpa ip6.arpa {
          pods insecure
          upstream
          fallthrough in-addr.arpa ip6.arpa
        }
        prometheus :9153
        forward . /etc/resolv.conf
        cache 30
        loop
        reload
        loadbalance
    }    
```
