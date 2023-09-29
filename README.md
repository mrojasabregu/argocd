# argocd


# Introduccion

## minikube

minikube es Kubernetes local y se centra en facilitar el aprendizaje y el desarrollo de Kubernetes.

https://minikube.sigs.k8s.io/docs/start/


### Installation

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

```bash
minikube start
```

```bash
minikube status
```

Pausar Kubernetes sin afectar las aplicaciones implementadas
```bash
minikube pause
```

Reanudar una instancia pausada
```bash
minikube unpause
```

Detener el clúster
```bash
minikube stop
```

Cambie el límite de memoria predeterminado (requiere reiniciar):

```bash
minikube config set memory 9001
```

Cree un segundo clúster que ejecute una versión anterior de Kubernetes
```bash
minikube start -p aged --kubernetes-version=v1.16.1
```

Elimine todos los clústeres de minikube
```bash
minikube delete --all
```

# ArgoCD 

## Instalacion

```bash
kubectl create namespace argocd
```

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Obtener la contraseña para el usuario admin
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

Port forward
```bash
kubectl port-forward service/argo-cd-argocd-server -n argocd 8080:443
```

Instalar la cli de argo
```bash
brew install argocd
```

Login de la cli 
```bash
argocd login localhost:8080
```

Actualizar password
```bash
argocd account update-password
```

Add repository
```bash
argocd repo add https://github.com/mrojasabregu/argocd.git
```

```bash
argocd proj create testing -d https://kubernetes.default.svc,testing -s https://github.com/mrojasabregu/argocd.git
```

````bash
argocd proj create system-admin -d https://kubernetes.default.svc,* -s '*'
````

````bash
kubectl create ns testing
````

````bash
argocd app create guestbook \
  --repo https://github.com/mrojasabregu/argocd.git \
  --revision main --path ./examples/guestbook \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace testing \
  --sync-policy automated \
  --project testing
````
  

Relanzar la App y sincronizar desde la CLI
````bash
argocd app sync guestbook
````
Si queremos saber el estatus de la App
````
argocd app get guestbook
````
Eliminar las Apps
````
argocd app delete guestbook
````
 Listar los Charts instalados
````
helm list -n argocd
````
Desistalar todo
````
helm uninstall argo-cd -n argocd
````
Eliminar los Namespaces 
````
kubectl delete ns argocd testing
````
Detener Minikube
```
minikube stop
```
