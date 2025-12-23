# Flux + Kubernetes Commands Reference

## 0. Delete Minikube clusters and start again

```powershell
minikube stop
minikube delete --all --purge
minikube start
minikube addons enable metrics-server
```

## 1. GitHub Environment Variables

```powershell
$env:GITHUB_TOKEN = "GITHUB_TOKEN"
$env:GITHUB_USER  = "GITHUB_USER"
$env:GITHUB_REPO  = "GITHUB_REPO"
```

---

## 2. Flux Pre-flight Check

```bash
flux check --pre
```

---

## 3. Bootstrap Flux with GitHub (Minikube)

```powershell
flux bootstrap github `
  --components-extra=source-watcher `
  --context=minikube `
  --owner=$env:GITHUB_USER `
  --repository=$env:GITHUB_REPO `
  --branch=minikube `
  --personal `
  --path=clusters/minikube
```

---

## 4. Reconcile Flux Resources Manually

```bash
flux reconcile source git flux-system -n flux-system
flux reconcile helmrelease task-tracker-front-end -n task-tracker
flux reconcile helmrelease task-tracker-back-end -n task-tracker
```

---

## 5. Watch Flux Resources

```bash
flux get all
```

```bash
flux get kustomizations -A
flux get helmrelease -A
```

---

## 6. External Artifacts (Flux Source Toolkit)

```bash
kubectl get externalartifacts.source.toolkit.fluxcd.io -n flux-system
kubectl get externalartifacts -n flux-system
```

```bash
kubectl describe artifactgenerator flux-system -n flux-system
```

---

## 7. Kubernetes Cluster Inspection

```bash
kubectl get secret -A
kubectl get pod -A
kubectl get pvc -A
kubectl get service -A
```

---

## 8. Render Helm Charts Locally

```bash
helm template task-tracker-back-end ./apps/base/task-tracker-back-end
helm template task-tracker-front-end ./apps/base/task-tracker-front-end
```

---

## 9. Test Backend Service from Inside Cluster

```bash
kubectl run test-client -n task-tracker --rm -it --image=curlimages/curl -- sh
```

```bash
curl -v http://task-tracker-back-end:8080/api/tasks
```

---

## 10. Local Hosts File Configuration (Windows)

**File:**

```
C:\Windows\System32\drivers\etc\hosts
```

**Entries:**

```text
127.0.0.1   front-end.example.com
127.0.0.1   back-end.example.com
```

---

## 11. Port Forward Istio External Gateway

```bash
kubectl port-forward -n istio-system svc/external-gw-istio 80:80
```

---

## 12. Summary

This command set supports:

- Flux bootstrap and reconciliation
- Helm-based application deployment
- Artifact and resource inspection
- In-cluster and local testing
- Istio gateway exposure

Use this document as a quick operational reference for the **task-tracker Flux + Helm + Istio setup**.
