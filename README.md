# building-the-best-kubernetes-test-cluster-on-macos

### colima (docker runtime. help running containers on macOS)
A docker runtime hosted in a vm,so containers actually running inside the vm.
note that the docker network is not on macOS.

setup colima:
  - create interface macOS:br100:192.168.106.1
  - create interface colima:col0:192.168.106.2 
  - create a route on macOS to route docker-net traffic to colima
  - allow incoming traffic from macOS:192.168.106:1 -> colima:docker-net(172.18/16)
```bash
$ brew install colima
$ colima start --network-address
$ colima list
$ sudo route -nv add -net 172.18 192.168.106.2
$ colima ssh ### this ssh to the vm. the next command should be executed inside the vm
   (colima-vm) $ sudo iptables -A FORWARD -s 192.168.106.1 -d 172.18.0.0/16 -i col0 -o br-3ca3442bb072 -p tcp -j ACCEPT
   (colima-vm) $ exit
```
### kind




### MetalLB : load-balancer implementation for bare metal Kubernetes

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/config/manifests/metallb-native.yaml
kubectl wait --namespace metallb-system \
                --for=condition=ready pod \
                --selector=app=metallb \
                --timeout=90s
```

### Kubeproxy
get commands from api-servers when SVC object created, and creates iptables chains to forward ports directly into pods.
to see it, you can check iptables of a node:

# login to the vm where all nodes are running as containers
$ colima ssh

  # show iptables nat rules of a node
  $ docker exec -ti chen-k8s-kind-worker iptables -nvL -t nat









### Links:
* [building-the-best-kubernetes-test-cluster-on-macos](https://opencredo.com/blogs/building-the-best-kubernetes-test-cluster-on-macos/)



