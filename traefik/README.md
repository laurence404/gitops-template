Configures [Traefik](https://github.com/traefik/traefik) (ingress controller) using [helm](https://github.com/traefik/traefik-helm-chart/blob/master/traefik/values.yaml)
* Log custom Cloudflare headers with the authenticated user details.
* Configures wildcard certificate for `*.local.yourdomain.com` provisioned by the [cert-manager](https://cert-manager.io/) chart.
* Enables [Gateway API](https://gateway-api.sigs.k8s.io/)

Traefik is configured to listen on the node's IP, so that DNS can be setup using terraform automatically. This is done using a `DaemonSet`, a `ClusterIP` service and `hostPort`. For clusters where a LoadBalancer Service can be provisioned (see Cilium config) this is unnecessary, although DNS will need to be manually updated to point to the LoadBalancer IP.
