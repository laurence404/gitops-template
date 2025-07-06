Creates the [MetalLB](https://metallb.io/) provisioner for Loadbalancer Services using a pool of IP addresses in the same subnet as the server. Configure IP range in [ipaddresspool.yaml](templates/ipaddresspool.yaml) - you may need to reconfigure your DHCP server to not allocate from the entire range leaving some IP addresses to be used by metallb.

Services of type Loadbalancer will be allocate one of the IP addresses in the range, e.g. for the Traefik ingress controller.
