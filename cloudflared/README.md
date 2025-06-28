Configures `cloudflared` to setup a tunnel to Cloudflare, which has been configured to receive traffic for `*.yourdomain.com`. All traffic is routed to Traefik which directs requests to matching `Ingress` resources. Uses Secret `tunnel` (either provisioned by terraform or manually).

`cloudflared` is configured to validate the JWT sent by Cloudflare, so even if you accidentally removed the authentication rules, the requests would be rejected.

Additionally the following hosts have been configured:
* `hello.youdomain.com` - simple hello world server to validate `cloudflared` is working
* `bastion.yourdomain.com` - bastion service, allowing you to use `cloudflared` on another device to forward traffic to arbitrary locations within your cluster/home network:

Example usage for connecting to Kubernetes API (sets up `127.0.0.1:6443` to forward via Cloudflare to `kubernetes.default:443` within your cluster):
```
$ cloudflared access tcp --hostname bastion.yourdomain.com --url 127.0.0.1:6443 --destination kubernetes.default:443
# Ensure ~/.kube/config contains server: https://127.0.0.1:6443
$ kubectl get pods
```
This will bring up a web browser to authenticate. Alternatively, create service tokens in Cloudflare Zero Trust -> Access -> Service auth, and add the following parameters: `--service-token-id <CF-Access-Client-Id> --service-token-secret <CF-Access-Client-Secret>`
