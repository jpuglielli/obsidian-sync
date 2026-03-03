# Kubernetes Ingress
A single entry point for all external HTTP traffic into the cluster, providing **Layer 7 (HTTP-aware) routing** to backend services.

## What it does

- **Host-based routing** — `api.example.com` → Service A, `app.example.com` → Service B
- **Path-based routing** — `example.com/api` → Service A, `example.com/app` → Service B
- **TLS termination** — handle HTTPS certs in one place
- **Single external IP** — one load balancer for many services (instead of one LoadBalancer Service per app)

## Ingress vs LoadBalancer Service

- A LoadBalancer Service gives one service an external IP (Layer 4, TCP/UDP level)
- An Ingress consolidates many services behind one IP and routes based on HTTP host/path
- Think of it as the Kubernetes-native equivalent of an Nginx reverse proxy config

## Key concept

An Ingress _resource_ defines the routing rules. An **Ingress Controller** (e.g. nginx-ingress, Traefik, AWS ALB controller) is what actually implements them.