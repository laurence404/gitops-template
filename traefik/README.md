Configures [Traefik](https://github.com/traefik/traefik) (ingress controller) using [helm](https://github.com/traefik/traefik-helm-chart/blob/master/traefik/values.yaml)
* Log custom Cloudflare headers with the authenticated user details.
* Configures wildcard certificate for `*.local.yourdomain.com` provisioned by the [cert-manager](https://cert-manager.io/) chart.
* Enables [Gateway API](https://gateway-api.sigs.k8s.io/)
* Use hostPort for exposing ingress from nodes IP
