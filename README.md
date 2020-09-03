# kustomize-remote-base-test

Este repositorio tiene como finalidad probar el funcionamiento de Argo o Flux con Kustomize utilizando bases remotas. ¿Son capaces estas herramientas de detectar los cambios en los recursos remotos sin tener que realizar nuevos commits en el repositorio que tienen sincronizado?

## Despliegue

Elegir una de las dos

### Argo

Creamos un namespace para Argo en nuestro cluster:

```
kubectl create namespace argocd
```

Instalamos Argo en el cluster
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Accedemos a la interfaz gráfica de Argo
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
y navegamos a [http://localhost:8080](http://localhost:8080). El usuario es `admin` y la contraseña es el output de este comando:
```
kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2
```

Desde aquí, lo que hay que hacer es crear una nueva aplicación en la interfaz de Argo y habrá que legir un `fork` de este repositorio como repositorio git. Las opciones de Kustomize se pueden dejar en blanco.

Una vez creada la aplicación, la tarea consiste en esperar a que se apliquen los recursos en el cluster, y realizar commits en el repositorio que contiene los parches (un `fork` de este) y en el repositorio que contiene las bases (un `fork` de [este](https://github.com/UrkoLekuona/kustomize-test)).

Como podremos comprobar, Argo solo detecta un cambio en la aplicación cuando ocurre un nuevo commit en el repositorio que está monitorizando directamente, el repositorio de los parches.
### Flux

TODO
