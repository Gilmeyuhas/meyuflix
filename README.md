# MeyuFlix – Self-Hosted Media Stack on K3s

This repository contains the declarative configuration for my **self-hosted home media environment**, deployed on a single-node [**K3s**](https://k3s.io/) Kubernetes cluster running on Ubuntu 24.04 Server.

Each application is either deployed using **Helm** (with `bjw-s/app-template`) or raw Kubernetes manifests under [`apps/`](./apps). This setup enables reproducible, declarative, and modular deployments — all isolated to my LAN.

## 📦 Stack Overview

| App           | Purpose                         | Deployment Type     | Config Path                          | Migrated |
|----------------|----------------------------------|----------------------|----------------------------------------|----------|
| [Bazarr](https://www.bazarr.media/)       | Subtitle downloader           | [Raw Manifests](./apps/bazarr)     | [`apps/bazarr`](./apps/bazarr)            | ✅ |
| Sonarr        | TV episode automation            | `bjw-s/app-template` | [`apps/sonarr`](./apps/sonarr)        | ❌ |
| Radarr        | Movie automation                 | `bjw-s/app-template` | [`apps/radarr`](./apps/radarr)        | ❌ |
| qBittorrent   | Torrent client (headless)        | `bjw-s/app-template` | [`apps/qbittorrent`](./apps/qbittorrent) | ❌ |
| Prowlarr      | Indexer manager                  | `bjw-s/app-template` | [`apps/prowlarr`](./apps/prowlarr)    | ❌ |
| [Jellyseerr](https://github.com/Fallenbagel/jellyseerr) | Media request interface     | [Raw Manifests](./apps/jellyseerr)  | [`apps/jellyseerr`](./apps/jellyseerr)    | ✅ |
| [Profilarr](https://github.com/saswatds/profilarr)     | Quality profile sync         | [Raw Manifests](./apps/profilarr)   | [`apps/profilarr`](./apps/profilarr)      | ✅ |
| [Homarr](https://github.com/ajnart/homarr)         | Home dashboard                 | [Raw Manifests](./apps/homarr)      | [`apps/homarr`](./apps/homarr)            | ❌ |
| [Flaresolverr](https://github.com/FlareSolverr/FlareSolverr) | CAPTCHA bypasser       | [Raw Manifests](./apps/flaresolverr) | [`apps/flaresolverr`](./apps/flaresolverr) | ❌ |
| [Dashdot](https://github.com/MauriceNino/dashdot)         | Node dashboard                 | [Raw Manifests](./apps/dashdot)     | [`apps/dashdot`](./apps/dashdot)          | ❌ |

## Current Status
This is a work in progress. My current non-kubernetes setup works. Some services are not yet migrated to this environment, and their files are not here, unless work has already begun on the transfer. This is a hobby, and I am not experienced in K8s, so do not take anything you see here as guides or best practices. If it helps you in any way, then I am more than happy to help, but know that this may not be the best way to do this.

## 🗂 Directory Structure

```bash
meyuflix-helm/
├── apps/
│   ├── bazarr/             # Raw manifests (Deployment, Service, Ingress, PV/PVC)
│   ├── profilarr/          # Same structure as bazarr
│   ├── jellyseerr/         # Same structure as bazarr
│   ├── sonarr/             # Helm values.yaml (app-template)
│   ├── radarr/             # Helm values.yaml (app-template)
│   └── ...
└── README.md
```

## 🚀 Deploying Apps

### 🔹 Helm-Based Apps

For apps using `bjw-s/app-template`:

```bash
helm repo add bjw-s https://bjw-s.github.io/helm-charts/
helm repo update

helm upgrade --install sonarr bjw-s/app-template \
  --namespace apps --create-namespace \
  -f apps/sonarr/values.yaml
```

### 🔹 Raw Manifest Apps

For Bazarr, Jellyseerr, Profilarr, etc., apply their YAMLs manually:

```bash
kubectl apply -f apps/bazarr/
```

Ensure persistent volumes and ingress hosts are properly configured.

## 🔒 Security & Access

This cluster is **not publicly accessible**. All apps are exposed internally using:

- **Traefik ingress controller**
- **`.local` hostnames** mapped via `/etc/hosts` or internal DNS
- **Cloudflare Tunnel** for secure remote access to select apps (e.g., Jellyseerr)

All services are locked to my LAN behind strict firewall rules. K3s is configured with secure defaults and RBAC.

## 🧪 Notes

- Most persistent apps use `hostPath` volumes bound to directories like `/opt/<app>/config`
- Apps are deployed in the `apps` namespace
- Work is underway to migrate remaining services from Docker Compose to K3s
- Future additions may use `Helmfile` for full-stack orchestration

---

## 👋 About Me

I’m a DevOps engineer and infrastructure builder who enjoys applying real-world practices — GitOps, declarative infra, secure service exposure — to my personal media stack.

If you’re curious how to containerize and run your entire home media setup on Kubernetes (for no reason at all), this repo may help!
