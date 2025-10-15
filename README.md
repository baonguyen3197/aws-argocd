```Aws ArgoCD Repository ```
============================
Get AWS K8s config
```bash
aws eks --region <region> update-kubeconfig --name <cluster_name>
```

============================
```bash
kubectl create namespace devops-tools
kubectl create namespace argocd
```

============================
```powershell
kubectl create secret docker-registry regcred `
  --docker-server=https://index.docker.io/v1/ `
  --docker-username=<DOCKERHUB_USER> `
  --docker-password=<DOCKERHUB_PASS> `
  --docker-email=<you@example.com> `
  -n devops-tools
```

============================
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

============================
```powershell
kubectl patch svc argocd-server -n argocd -p '{\"spec\": {\"type\": \"LoadBalancer\"}}'
kubectl patch svc argocd-server -n argocd -p '{\"metadata\": {\"annotations\": {\"service.beta.kubernetes.io/aws-load-balancer-type\": \"nlb\", \"service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled\": \"true\"}}}'
kubectl get svc -n argocd
```

============================
```powershell
$encodedPassword = kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}"

[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedPassword))
```

============================
```bash
kubectl -n argocd edit deployment argocd-server
kubectl -n argocd edit configmap argocd-rbac-cm
```
