# smarty-home
This helm chart is to manage a smart home

## K8S flavor
When running in a microk8s cluster, use the following to enable the ingress traffic to the machine:
```
microk8s enable dashboard
HOST_IP=$(hostname  -I | cut -f1 -d' ')
microk8s enable dns metallb:$HOST_IP-$HOST_IP
```

```
multipass exec microk8s-vm -- sudo /snap/bin/microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
```

On multipass VM to route traffic:
```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination <yourhostip>:80
```