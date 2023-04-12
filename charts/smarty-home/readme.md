# Things to know

## Traefik ingress
This chart assumes that traefik is installed and creates ingress routes based on the traefik ingressroute resource.
If traefik is not installed or ingress routes need to be disabled, set the following line to false in the values.yaml file:
`enableTraefikRoutes: true`

If you are running the traefik dashboard, it needs to be disabled or the ingress routes will get confused on /api. Both Traefik and Frigate use the /api route

## Exposed paths
`/nvr`
`/assistant`

## Default values
the following values are defaultsed and can be overwritten manually

`configCapacity: 10Gi`
each service will be allocated the same capacity. Divide the total config capacity by 6 (or the number of services that will be deployed)

`mediaCapacity: 15Ti`
the entire capacity for the media directory in NFS

`timezone: Etc/UTC`
Localization for specific timezones will automatically apply to all the linuxserver.io images but UTC is universal so... thats what it is ;)

`PGID: 0`
`PUID: 0`
this is the `root` user and `root` group in linux so if the NFS has security configurations, this should be set appropriately
