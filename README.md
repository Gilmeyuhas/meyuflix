# MeyuFlix – Self-Hosted Media Stack on K3s

This repository contains the declarative configuration for my **self-hosted home media environment**, deployed on a single-node [**K3s**](https://k3s.io/) Kubernetes cluster running on Ubuntu 24.04 Server.

Each application is either deployed using **Helm** (e.g. official Helm charts or other reputable charts) or raw Kubernetes manifests under [`apps/`](./apps) and [`utils/`](./utils). This setup enables reproducible, declarative, and modular deployments — all isolated to my LAN.

---

## 📦 Stack Overview

| App                                                                                          | Purpose                          | Deployment Type               | Config Path                                | Migrated/Implemented |
|-----------------------------------------------------------------------------------------------|----------------------------------|-------------------------------|--------------------------------------------|----------------------|
| [Bazarr](https://www.bazarr.media/)                                                           | Subtitle downloader              | Raw Manifests                 | [`apps/bazarr`](./apps/bazarr)             | ✅ |
| [Sonarr](https://sonarr.tv/)                                                                  | TV episode automation   | Official Helm Chart           | [`apps/sonarr`](./apps/sonarr)             | ❌ |
| [Radarr](https://radarr.video/)                                                               | Movie automation   | Official Helm Chart           | [`apps/radarr`](./apps/radarr)             | ❌ |
| [qBittorrent](https://www.qbittorrent.org/)                                                   | Torrent client (headless)   | Official Helm Chart           | [`apps/qbittorrent`](./apps/qbittorrent)   | ❌ |
| [Prowlarr](https://wiki.servarr.com/prowlarr)                                                 | Indexer manager   | Official Helm Chart           | [`apps/prowlarr`](./apps/prowlarr)         | ❌ |
| [ErsatzTV](https://ersatztv.org/)                                                             | Virtual live TV channels         | Raw Manifests                 | [`apps/ersatztv`](./apps/ersatztv)         | ✅ |
| [Jellyseerr](https://github.com/Fallenbagel/jellyseerr)                                       | Media request interface          | Raw Manifests                 | [`apps/jellyseerr`](./apps/jellyseerr)     | ✅ |
| [Kometa](https://www.kometa.wiki/)                                                             | Library metadata overlays & collections | Raw Manifests                 | [`apps/kometa`](./apps/kometa)             | ✅ |
| [Profilarr](https://github.com/saswatds/profilarr)                                            | Quality profile sync             | Raw Manifests                 | [`apps/profilarr`](./apps/profilarr)       | ✅ |
| [Agregarr](https://github.com/agregarr/agregarr)                                              | Plex recommendations          | Raw Manifests                 | [`apps/agregarr`](./apps/agregarr)       | ✅ |
| [Homarr](https://github.com/ajnart/homarr)                                                    | Home dashboard                   | Raw Manifests                 | [`apps/homarr`](./apps/homarr)             | ✅ |
| [Kubernetes Dashboard](https://github.com/kubernetes/dashboard)                               | Dashboard for K8s                | Raw Manifests                 | [`apps/kubernetes-dashboard`](./apps/kubernetes-dashboard) | ✅ |
| [Dashdot](https://github.com/MauriceNino/dashdot)                                             | Node metrics dashboard (used in Homarr) | Raw Manifests         | [`apps/dashdot`](./apps/dashdot)           | ✅ |
| [Speedtest Tracker](https://github.com/henrywhitaker3/Speedtest-Tracker)                     | Internet speed monitor           | `soblivionscall` Helm Chart   | [`utils/speedtest-tracker`](./utils/speedtest-tracker) | ✅ |
| [Tdarr](https://github.com/HaveAGitGat/Tdarr)                                                 | Conditional media transcoding/remuxing | `haveagitgat` Helm Chart | [`apps/tdarr`](./apps/tdarr)               | ✅ |
| [yt-dlp-webui](https://github.com/MaxelAmador/yt-dlp-web-ui)                                 | Download videos using yt-dlp with a Web UI | Raw Manifests        | [`utils/yt-dlp`](./utils/yt-dlp)           | ✅ |

---

## Current Status

This is a work in progress. My current non-Kubernetes setup works. Some services are not yet migrated to this environment, and their files are not here unless work has already begun on the transfer. This is a hobby, and I am not experienced in K8s, so do not take anything you see here as guides or best practices. If it helps you in any way, then I am more than happy to help, but know that this may not be the best way to do this.

---

## 🗂 Directory Structure

```bash
meyuflix/
├── apps/
│   ├── agregarr/               # Raw manifests
│   ├── bazarr/                 # Raw manifests (Deployment, Service, Ingress, PV/PVC)
│   ├── dashdot/                # Raw manifests
│   ├── ersatztv/               # Raw manifests
│   ├── homarr/                 # Raw manifests
│   ├── jellyseerr/             # Raw manifests
│   ├── kometa/                 # Raw manifests (Kometa + Kometa Quickstart)
│   ├── kubernetes-dashboard/   # Raw manifests
│   ├── profilarr/              # Raw manifests
│   ├── prowlarr/               # Helm values.yaml (TBC)
│   ├── qbittorrent/            # Helm values.yaml (TBC)
│   ├── radarr/                 # Helm values.yaml (TBC)
│   ├── sonarr/                 # Helm values.yaml (TBC)
│   └── tdarr/                  # Helm values + manifests
├── namespaces/
│   ├── apps-namespace.yaml
│   └── utils-namespace.yaml
├── utils/
│   ├── speedtest-tracker/      # Helm values.yaml (soblivionscall)
│   └── yt-dlp/                 # Raw manifests
└── README.md
```

---

## 🚀 Deploying Apps

### 🔹 Helm-Based Apps

For Sonarr, Radarr, qBittorrent, and Prowlarr (official charts):

```bash
helm repo add arr-dev https://arr-dev.github.io/charts/
helm repo update

helm upgrade --install sonarr arr-dev/sonarr \
  --namespace apps --create-namespace \
  -f apps/sonarr/values.yaml
```

For Tdarr:

```bash
helm repo add haveagitgat https://haveagitgat.github.io/helm-charts/
helm repo update

helm upgrade --install tdarr haveagitgat/tdarr \
  --namespace apps --create-namespace \
  -f apps/tdarr/values.yaml
```

For Speedtest Tracker:

```bash
helm repo add soblivionscall https://soblivionscall.github.io/charts/
helm repo update

helm upgrade --install speedtest-tracker soblivionscall/speedtest-tracker \
  --namespace utils --create-namespace \
  -f utils/speedtest-tracker/values.yaml
```
### 🔹 Raw Manifest Apps

For Bazarr, ErsatzTV, Homarr, Jellyseerr, Kometa, Profilarr, Dashdot, Kubernetes Dashboard, etc., apply their YAMLs manually:

```bash
kubectl apply -f apps/bazarr/
```

Ensure persistent volumes and ingress hosts are properly configured.

---

## 🔒 Security & Access

This cluster is **not publicly accessible**. All apps are exposed internally using:

- **Traefik ingress controller**
- **`.home` hostnames** mapped via LAN-wide `dnsmasq` or router DNS overrides

All services are locked to my LAN behind strict firewall rules. K3s is configured with secure defaults and RBAC.

---

## 🧪 Notes

- Most persistent apps use `hostPath` volumes bound to directories like `/opt/<app>/config`
- Applications run in the `apps` namespace, with supporting tools in `utils`
- Work is underway to migrate remaining services from Docker Compose to K3s
- Future additions may use `Helmfile` for full-stack orchestration

---

## 👋 About Me

I’m a DevOps engineer and infrastructure builder who enjoys applying real-world practices — GitOps, declarative infra, secure service exposure — to my personal media stack.

If you’re curious how to containerize and run your entire home media setup on Kubernetes (for no reason at all), this repo may help!
