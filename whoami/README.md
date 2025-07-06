A [simple echo service](https://github.com/traefik/whoami) at `whoami.yourdomain.com` and `whoami.local.yourdomain.com`. Allows you to inspect headers being set by Cloudflare and Traefik.

This uses the more modern [Gateway API](https://gateway-api.sigs.k8s.io/) to expose the app. The ArgoCD UI won't give you a hyperlink to your app with HTTPRoute resources, unlike [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/). If you want this, replace with an Ingress resource, like [ingress.yaml](../argocd/templates/ingress.yaml).
