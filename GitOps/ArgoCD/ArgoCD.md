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
GIT_URL=''
GIT_DIR=''
argocd app create guestbook --repo "${GIT_URL}" --path "${GIT_DIR}" --dest-server "${USER_NAME}" --dest-namespace gitops
```

## ArgoCD 활용 | Kustomize
