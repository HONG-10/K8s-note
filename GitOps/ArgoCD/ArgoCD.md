# ArgoCD

> ArgoCD는 GitOps의 구현체

## ArgoCD 특징
- 변경사항 감지 후 자동 반영
- Self-healing
- 이상 탐지

## ArgoCD Provisioning
```sh
# ArgoCD Namespace
kubectl create namespace argocd
kubectl get ns

# ArgoCD Install
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/ha/install.yaml

# ArgoCD CLI Install
VERSION=v2.0.0; curl –sL –o argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
chmod +x argocd
mv argocd /usr/local/bin/argocd
argocd -hKubernetes  +  ArgoCD  -  

# ArgoCD Service 
## Default | ClisterIP
kubectl get svc argocd-server -n argocd

## Public Cloud | LoadBalancer
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

## Bare Metal | NodePort
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

# ArgoCD Secret
## 초기 비밀번호 확인
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

## 비밀번호 환경변수 등록
tee ~/.bash_profile << EOF
export ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
EOF

source ~/.bash_profile

echo $ARGO_PWD

## ArgoCD Login | User : admin
argocd login [IP_ADDRESS]

## ArgoCD Logout
argocd logout [IP_ADDRESS]
```

## ArgoCD Git 연동
```sh
argocd app create [APP_NM] --repo [REPOSITORY_URL] --path [REPOSITORY_PATH] --dest-server [SERVER_URL] --dest-namespace [NAMESPACE_NM]

    # Create a directory app
    argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --directory-recurse

    # Create a Jsonnet app
    argocd app create jsonnet-guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path jsonnet-guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --jsonnet-ext-str replicas=2

    # Create a Helm app
    argocd app create helm-guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path helm-guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --helm-set replicaCount=2

    # Create a Helm app from a Helm repo
    argocd app create nginx-ingress --repo https://kubernetes-charts.storage.googleapis.com --helm-chart nginx-ingress --revision 1.24.3 --dest-namespace default --dest-server https://kubernetes.default.svc

    # Create a Kustomize app
    argocd app create kustomize-guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path kustomize-guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --kustomize-image gcr.io/heptio-images/ks-guestbook-demo:0.1

    # Create a app using a custom tool:
    argocd app create ksane --repo https://github.com/argoproj/argocd-example-apps.git --path plugins/kasane --dest-namespace default --dest-server https://kubernetes.default.svc --config-management-plugin kasane
```

## ArgoCD 활용 | Kustomize

## ArgoCD 활용 | Helm
