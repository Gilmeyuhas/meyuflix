# MeyuFlix – Self-Hosted Media Stack on K3s

This repository contains the declarative configuration for my **self-hosted home media environment**, deployed on a single-node [**K3s**](https://k3s.io/) Kubernetes cluster running on Ubuntu 24.04 Server.

Each application is managed using **Helm** and defined by a version-controlled `values.yaml` file under `apps/`. This setup enables reproducible, secure deployments — all isolated to my LAN.

## Current Status

This is a work in progress.
My current non-kubernetes setup work.
Some services are not yet migrated to this environment, and their files are not here, unless work has already begun on the transfer.
This is a hobby, and I am not experienced in K8s, so do not take anything you see here as guides or best practeces.

## 🔧 Stack Overview

| App         | Purpose                      | Chart Source                    |
|-------------|------------------------------|----------------------------------|
| [Bazarr](https://www.bazarr.media/)         | Subtitle downloader              | `bjw-s/app-template`             |
| Sonarr      | TV episode automation         | `bjw-s/app-template`             |
| Radarr      | Movie automation              | `bjw-s/app-template`             |
| qBittorrent | Torrent client (headless)     | `bjw-s/app-template`             |
| Prowlarr    | Indexer manager               | `bjw-s/app-template`             |
| Jellyseerr  | Media request interface       | Custom chart or raw manifest     |
| Profilarr   | Quality profile sync          | Custom chart or raw manifest     |
| Homarr      | Home dashboard                | Custom chart or raw manifest     |
| Flaresolverr| Cloudflare CAPTCHA bypasser   | Raw manifest                     |
| Dashdot     | Node dashboard                | Raw manifest or `bjw-s` chart    |

## 🗂 Directory Structure

```bash
home-cluster/
├── apps/
│   ├── bazarr/             # Helm values.yaml for Bazarr
│   ├── radarr/
│   ├── sonarr/
│   └── ...
└── README.md
````

## 🚀 Deployment

> You can apply each app independently using Helm and the `app-template` chart:

```bash
helm repo add bjw-s https://bjw-s.github.io/helm-charts/
helm repo update

helm upgrade --install bazarr bjw-s/app-template \
  --namespace media --create-namespace \
  -f apps/bazarr/values.yaml
```

> Or orchestrate the whole stack using [Helmfile](https://github.com/helmfile/helmfile) (to be implemented in the future).

## 🔒 Security & Scope

This cluster is **not publicly exposed**. All ingress routes and services are locked to my local LAN and protected by firewall and DNS-level controls. K3s is configured with strict default permissions and token-based authentication.

---

## 👋 About Me

I’m a DevOps engineer and infrastructure builder who enjoys bringing production-grade workflows into personal projects. This repo shows how I apply real-world practices — version control, Helm packaging, GitOps structure — to solve practical problems at home.

