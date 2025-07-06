Configures `cloudflared` to setup a tunnel to Cloudflare, using Secret `tunnel` (either provisioned by terraform or manually). 

`cloudflared` receives traffic for `*.yourdomain.com` and apart from the exceptions below, all traffic is routed to Traefik which directs requests to matching `Ingress` and `HTTPRoute`. The JWT set by Cloudflare is validated, so even if you accidentally removed the authentication rules in Zero Trust, the requests would be rejected by `cloudflared`.

* `hello.youdomain.com` - simple hello world server to validate `cloudflared` is working
* `argocd.yourdomain.com/api/webhook` and `argocd-applicationset.yourdomain.com/api/webook` - to allow incoming webhooks from github, which lack a JWT
* `bastion.yourdomain.com` - bastion service, allowing you to use `cloudflared` on another device to forward traffic to arbitrary locations within your cluster/home network:

Example usage for connecting to Kubernetes API (sets up `127.0.0.1:6443` to forward via Cloudflare to `kubernetes.default:443` within your cluster):
```
$ cloudflared access tcp --hostname bastion.yourdomain.com --url 127.0.0.1:6443 --destination kubernetes.default:443
# Ensure ~/.kube/config contains server: https://127.0.0.1:6443
$ kubectl get pods
```
This will bring up a web browser to authenticate. Alternatively, create service tokens in Cloudflare Zero Trust -> Access -> Service auth, and add the following parameters: `--service-token-id <CF-Access-Client-Id> --service-token-secret <CF-Access-Client-Secret>`
