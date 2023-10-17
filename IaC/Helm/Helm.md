# Helm

[Helm 공식 홈페이지](https://helm.sh/)
[Helm 공식 문서](https://helm.sh/ko/docs/)
[bitnami Helm Charts](https://bitnami.com/stacks/helm)


> Kubernetes package manager

## Helm Provisioning
```sh
# Helm Install
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh

# Helm bash completion 등록
$ yum -y install bash-completion
$ source <(helm completion bash)
$ echo "source <(helm completion bash)" >> ~/.bashrc
```
