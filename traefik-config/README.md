Configures [Traefik](https://docs.k3s.io/networking/networking-services#traefik-ingress-controller) (ingress controller) using [HelmChartConfig](https://docs.k3s.io/helm?_highlight=helmchartconfi#customizing-packaged-components-with-helmchartconfig)
* Log custom Cloudflare headers with the authenticated user details.
* Configures wildcard certificate for `*.local.yourdomain.com` provisioned by the [cert-manager](https://cert-manager.io/) chart.
* Enables [Gateway API](https://gateway-api.sigs.k8s.io/)
* Preserve source IP of clients in logs and headers by configuring [service load balancer](https://docs.k3s.io/networking/networking-services#service-load-balancer)
