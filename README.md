## K8S DNS
- `dns-python-test.yaml`: Sample python app
- `curl-client.yml`: curl service is  to know the IPs of the service’s endpoints and be dependent on the ephemeral nature of Kubernetes pods.

#### Debugging
- nslookup command failure:
  ```bash
  $ kubectl exec -ti busybox -- nslookup
  kubernetes.default

  Server:    10.0.0.10
  Address 1: 10.0.0.10
  nslookup: cant resolve kubernetes.default
  ```
  Check `/etc/resolv.conf` inside the pod for which nslookup failed.

  ```bash
  $ kubectl exec <pod_name> cat /etc/resolv.conf
  ```
- Check the kube-dns kubedns/coredns pods are running.
  ```bash
  $ kubectl get pods -n=kube-system

  # Output
  NAME                                         READY     STATUS    RESTARTS   AGE
  kube-dns-86f4d74b45-zh5v9                    3/3       Running   0          4d
  ```
- If Pods are running check the service
  ```bash
  $ kubectl get svc -n=kube-system
  NAME       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)         AGE
  kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP   4d
  ```
- We can also check the dns endoints are exposed.
  ```bash
  $ kc get ep kube-dns -n=kube-system
  NAME       ENDPOINTS                     AGE
  kube-dns   10.1.0.123:53,10.1.0.123:53   4d
  ```
