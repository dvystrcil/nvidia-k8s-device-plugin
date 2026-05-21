# nvidia-k8s-device-plugin

Helm values for the upstream [NVIDIA k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin) chart, deployed into this cluster via ArgoCD.

Sister repo to [`rocm-k8s-device-plugin`](https://github.com/dvystrcil/rocm-k8s-device-plugin) — same multi-source pattern (Helm chart from upstream + values from here).

## What this exposes

`nvidia.com/gpu: 1` as a schedulable resource on any node labeled `homelab/role=small-model-gpu`. Today that's only `k8s-node-03` (GTX 970 via Proxmox PCIe passthrough — see [homelab#165](https://github.com/dvystrcil/homelab/issues/165)).

## How a workload claims it

```yaml
spec:
  nodeSelector:
    homelab/role: small-model-gpu
  containers:
  - name: ollama
    resources:
      limits:
        nvidia.com/gpu: 1
```

## Argo App

Lives in `dvystrcil/argocd-projects/k8s-device-plugins/nvidia-k8s-device-plugin.yaml`.
