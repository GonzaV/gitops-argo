# gitops-argo

## Qué es este repositorio?

Este repositorio contiene la documentación y manifiestos básicos para probar de manera local Argo CD y entender los fundamentos de GitOps.

## Contenido

1. [Conocimientos teóricos necesarios](#conocimientos-teóricos-necesarios)
2. [Herramientas necesarias](#herramientas-necesarias)
3. Paso a paso 
   - [Sincronización básica de una Aplicación](#sincronización-básica-de-una-aplicación)

## Conocimientos teóricos necesarios

Antes de empezar, es recomendable leer la [introducción a GitOps](/docs/gitops.md)

## Herramientas necesarias

Para seguir el [paso a paso](#paso-a-paso), se necesita tener instaladas las siguientes herramientas en tu equipo.

> Estas y el restante de las instrucciones estan pensadas para entornos Linux o MacOs. Se asume que se cuenta con Git instalado.

1. **Docker**

    Es necesario tener un entorno de ejecución de contenedores.
    En esta práctica se usará [Podman](https://podman.io/), pero existen alternativas como [Docker Desktop](https://docs.docker.com/desktop/) o [Colima](https://github.com/abiosoft/colima).

2. **Cluster local de Kubernetes**

    En esta práctica se usará [Minikube](https://minikube.sigs.k8s.io/docs/), pero existen [Kind](https://kind.sigs.k8s.io/) como alternativa viable.

3. **Kubectl**

    [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) es el cliente oficial de Kubernetes. Será neceario para aplicar cambios manuales en el cluster.

### Verificación rápida

Abrir una terminal y correr los siguientes comandos:

> colima status

> minikube status

> kubectl get nodes

Si todos responden sin errores, está todo listo para comenzar.
    
## Paso a paso

### Instalación y preparación de Argo CD

// TODO mejoras: asegurarnos de estar trabajando en el cluster local con kubectl config get-contexts. En "Creación y Sincronización de una Aplicación" se crea manualmente la aplicacion de argo por simplicidad, asegurarse de comentar que hay alternativas, ya que esto tambien requiere permisos que no deberian tener todos los devs.

Como primer paso, es necesario crear el namespace `argocd` y aplicar en el los recursos y servicios de Argo.

Para eso ejecutamos en una consola:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Kubectl va a listarnos los recursos nativos de Kubernetes que aplico bajo el namespace `argocd` que creamos.

Confirmamos que los pods de Argo estan corriendo:

```bash
kubectl get pods -n argocd 
```
![argo-cd-pods](./docs/images/argo-pods.png)

Con Argo levantado, lo siguiente es exponer su API y UI. No existe una única forma de hacer esto, pero la más sencilla, sin aplicar otras configuraciones ni recursos de k8s, es exponer el `service argocd-server` con el comando `port-forward`.

En una **nueva instancia de nuestra consola** corremos el siguiente comando:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Es importante hacerlo en una nueva consola ya que este proceso es continuo y no nos va a permitir ejecutar otros comandos.

Si todo salió bien, ya es posible ingresar a la UI en http://localhost:8080 con cualquier navegador.

![argo-login](./docs/images/argo-login.png)

Argo genera un usuario `admin` con una contraseña inicial que podemos obtener con:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath='{.data.password}' | base64 -d
```

Estas son las credenciales que vamos a usar para ingresar a la UI expuesta en localhost.

### Creación y Sincronización de una Aplicación

En este punto, nuestro cluster de kunbernetes esta corriendo de forma local, Argo CD esta deployado en ese ambiente y su UI accesible en localhost:8080. Es momento de crear una `Aplicación`.

Las aplicaciones de Argo son la representación de nuestros servicios deployados o por deplyoar y brindan, entre otras cosas, dos piezas centrales de información: la fuente o referencia al estado deseado (nuestro repositorio) y el cluster destino.

Por simplicidad del ejemplo, vamos a hacer una creación manual haciendo uso del [CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) `Application` de Argo.

En este mismo repositorio se encuentra el archivo [nginx-app.yml](./apps/nginx-app.yml). Vamos a aplicar el mismo de esta forma desde el directorio en donde se encuentra (o usando un path relativo):

```bash
  kubectl apply -f nginx-app.yaml
```
