Deploy [Cilium](https://cilium.io) as a CNI to enforce [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/) and to implement LoadBalancer services.

In order to use LoadBalanacer service, update `templates/ciliumloadbalancerippool.yaml` to contain IP addresses on the same subnet as your server but outside of the range allocated by your DHCP server.
