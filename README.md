# kubernetes-homelab

A set of configuration files I use to run my homelab on Kubernetes.

## To Do
* [ ] Use security context to allow containers to use non-root users
* [ ] Use containers that exclusively use non-root users

## Services
* Authelia - single sign-on with multi-factor
* Cert Manager - retrieves Let's Encrypt certificates for exposed services
* Dynamic DNS - updates a DNS record with my home IP, to avoid paying for a static IP
* Nginx Ingress - expose services to the internet, and internally
* MetalLB - assign load balancer services IPs internally on my network
* Monitoring - InfluxDB, Grafana, Telegraf, Speedtest.net and UDM data
* Plex - for playing back media I've ripped off DVDs

## Quirks
### Sealed Secrets
I'm using [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets/) to store secrets on GitHub without exposing credentials.

### Nginx Ingress
I run two separate deployments of the Nginx Ingress controller. One is for internal traffic, and the other is for external traffic.

Each controller has a `LoadBalancer` service, which are assigned IPs by [MetalLB](https://metallb.universe.tf/). This allows me to port-forward
one LB for external traffic, while leaving the other for accessing services internally.

Each deployment has it's own Ingress class, `nginx` and `nginx-internal`. When creating a new Ingress, I can choose whether it should be exposed
internally or externally.

In a nutshell: external ingresses are available over the internet, but internal ingresses are only available via Wireguard (a VPN). 
