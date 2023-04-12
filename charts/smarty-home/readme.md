# Things to know

## Networking and admin
To get the token for the k8s dashboard, run the following snip:
```
token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s kubectl -n kube-system describe secret $token
```

## Exposed paths

## Default values
the following values are defaultsed and can be overwritten manually

