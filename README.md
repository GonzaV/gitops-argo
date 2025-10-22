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

### Sincronización básica de una Aplicación

TBC