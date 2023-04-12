# smarty-home
This helm chart is to manage a smart home


## Pre-requisites
- Some flavor of kubernetes (microk8s, rancher desktop, docker desktop, etc)
- Appropriate configuration of ~/.kube/config to hit the cluster
- Kubectl and helm commands should be mapped appropriately (EX alias `microk8s kubectl` to `kubectl`)
- (optional) 2 NFS share paths
    - media
    - config

## How to use this repo
Depending on the platform, either a bash or powershell script is recommended. The default values files does not have real values but instead a list of variables that need to be subbed in. This is an example in powershell but something simliar will work in bash:

```
pushd (Join-path $PSScriptRoot "/charts/smarty-home/")
$filename="values.yaml"
# update these values to set the helm chart values file
# nfs
$server= ""
$config= ""
$localPath= ""
$media= ""

(Get-Content $filename).Replace('YOUR_NFS_SERVER',$server) | Set-Content $filename
(Get-Content $filename).Replace('YOUR_CONFIG_PATH',$config) | Set-Content $filename
(Get-Content $filename).Replace('YOUR_LOCAL_STORAGE',$localPath) | Set-Content $filename
(Get-Content $filename).Replace('YOUR_MEDIA_PATH',$media) | Set-Content $filename

if(-not (test-path Chart.lock))
{
    helm dependency build
}
helm upgrade -i smartyhome .
popd
```

Also... powershell is really easy to install.
Mac:

`brew install powershell`

Linux:

`snap install powershell`

Then run the command `pwsh` to start a powershell session.

## microk8s notes
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