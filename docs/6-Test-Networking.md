# Test Networking

## port-forwarding, exec check

```bash
kubectl create deployment nginx --image=nginx --replicas=2
kubectl port-forward deployment/nginx 8080:80
kubectl exec -it nginx-<pod-name> -- /bin/bash
```

check:
```
curl --head http://0.0.0.0:8080
```

result:
```bash
HTTP/1.1 200 OK
Server: nginx/1.27.0
Date: Sun, 16 Jun 2024 05:20:37 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Tue, 28 May 2024 13:22:30 GMT
Connection: keep-alive
ETag: "6655da96-267"
Accept-Ranges: bytes
```

## deploy netshoot pod for testing communication between pods

```bash
kubectl apply -f https://k8s.io/examples/admin/dns/netshoot.yaml
```

## check the pod

```bash
kubectl get po -l app=netshoot -o wide
NAME             READY   STATUS    RESTARTS   AGE     IP               NODE                   NOMINATED NODE   READINESS GATES
netshoot-2r44s   1/1     Running   0          6m14s   192.168.22.193   calico-control-plane   <none>           <none>
netshoot-hmd8m   1/1     Running   0          4m53s   192.168.18.69    calico-worker2         <none>           <none>
netshoot-njrdp   1/1     Running   0          5m27s   192.168.9.134    calico-worker          <none>           <none>

```

```bash
kubectl exec -it netshoot-2r44s -- /bin/bash
netshoot-2r44s:~# nslookup nginx.default.svc.cluster.local
Server:		10.96.0.10
Address:	10.96.0.10#53

Name:	nginx.default.svc.cluster.local
Address: 10.96.93.157

netshoot-2r44s:~# curl --head nginx.default.svc.cluster.local:8080
HTTP/1.1 200 OK
Server: nginx/1.27.0
Date: Sun, 16 Jun 2024 05:23:48 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Tue, 28 May 2024 13:22:30 GMT
Connection: keep-alive
ETag: "6655da96-267"
Accept-Ranges: bytes

netshoot-2r44s:~# ping -c 3 192.168.18.69
PING 192.168.18.69 (192.168.18.69) 56(84) bytes of data.
64 bytes from 192.168.18.69: icmp_seq=1 ttl=62 time=1.47 ms
64 bytes from 192.168.18.69: icmp_seq=2 ttl=62 time=0.181 ms
64 bytes from 192.168.18.69: icmp_seq=3 ttl=62 time=0.330 ms

--- 192.168.18.69 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2025ms
rtt min/avg/max/mdev = 0.181/0.660/1.470/0.575 ms
netshoot-2r44s:~# ping -c 3 192.168.9.134
PING 192.168.9.134 (192.168.9.134) 56(84) bytes of data.
64 bytes from 192.168.9.134: icmp_seq=1 ttl=62 time=1.47 ms
64 bytes from 192.168.9.134: icmp_seq=2 ttl=62 time=0.379 ms
64 bytes from 192.168.9.134: icmp_seq=3 ttl=62 time=0.273 ms

--- 192.168.9.134 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2034ms
rtt min/avg/max/mdev = 0.273/0.705/1.465/0.538 ms
```
