# GitOps: de la Infraestructura como Código a la Operación Continua

## Introducción

IaC (Infrastructure as Code) y sus evoluciones —Configuration as Code, Security as Code, Network as Code, etc.— nos permiten definir nuestra infraestructura y configuraciones mediante código o archivos de configuración.

Esto es una gran ventaja porque nos brinda reproducibilidad, trazabilidad y control de versiones sobre los entornos de infraestructura.
Sin embargo, una vez que tenemos estos archivos y configuraciones (por ejemplo, ficheros de Terraform o recursos de Kubernetes en formato .yaml), surge una pregunta importante:

**¿Cómo los aplicamos realmente?**

## El enfoque tradicional

Una opción común es usar herramientas y comandos como:

```bash
kubectl apply
terraform apply
```

Pero esto implica conectarse directamente a la infraestructura y aplicar los cambios desde nuestros ambientes locales.
Este enfoque tiene varias desventajas:
  - ❌ No hay un registro claro de quién hizo los cambios.
  - ❌ Todos los operadores necesitan acceso directo a la infraestructura (clúster, cuenta AWS, etc.).
  - ❌ Los errores se detectan solo al momento de aplicar los cambios.
  - ❌ No existe un mecanismo sencillo de rollback.

En este punto, podemos estar aplicando IaC, pero el proceso sigue siendo manual e ineficiente especialmente en grandes organizaciones o equipos de trabajo.

Las prácticas de GitOps son una respuesta a estos escenarios.

## ¿Qué es GitOps?

>[Definición del equipo de GitLab] GitOps es un marco de operación que toma las prácticas recomendadas de DevOps que se usan para el desarrollo de aplicaciones, como el control de versiones, la colaboración, el cumplimiento y la CI/CD, y las aplica a la automatización de la infraestructura.

GitOps trata al código de infraestructura y al de aplicaciones de la misma manera. Todos los archivos relacionados con IaC están alojados en un repositorio Git, lo que nos brinda de inmediato:
- Control de versiones.
- Colaboración entre equipos.
- Integración de pruebas automáticas (linting, testing) en pipelines de CI.
- Mecanismo de rollback mediante git revert o git checkout.

Sin embargo, esto no termina de resolver todos los problemas planteados:
- ❌ El registro claro de quién hizo los cambios no esta asegurado: podría aplicar modificaciones a mi infraestructura sin pasar por el repositorio.
- ❌ Los desarrolladores todavía necesitan acceso directo a la infraestructura sin otro mecanismo de despliegue automático.

Para completar la propuesta, GitOps incorpora la automatización del despliegue (CD) como parte central del flujo.

## GitOps en acción

Un concepto fundamental sobre el que GitOps se para es el Single Source of Truth (SSOT):
**Todo lo que está en la rama principal del repositorio representa el estado real deseado del sistema.**

Esto significa que:
- Si el repositorio dice que debe existir un Deployment en Kubernetes, ese recurso debe existir en el clúster.
- Si alguien modifica algo directamente en el entorn por cualquier otro medio que no sea una modificación del SSOT, el sistema GitOps debería detectarlo y corregirlo.

Para lograr esto, se necesita un mecanismo que sincronice continuamente el estado real del entorno con el estado del repositorio. Podemos clasificar a estos mecanismos en dos tipos: Pull-based y Push-based.

### Push-based

En este modelo, los propios pipelines (por ejemplo, Jenkins, GitLab CI, GitHub Actions) aplican los cambios ejecutando comandos los mencionados:

```bash
kubectl apply
terraform apply
```

Este modelo tiene algunos problemas:
- No existe verificación continua del estado real. Para que la sincronización ocurra es necesario correr nuevamente el pipeline.
- De la misma manera, es difícil detectar o corregir cambios manuales hechos fuera del repositorio.
- Los pipelines requieren permisos de alto nivel (“god mode”).


### Pull-based

En este enfoque se introduce un nuevo actor: el **Operador u Operator**.

Este componente se despliega directamente dentro del entorno y se encarga de comparar de forma continua el estado deseado (repositorio Git) con el estado actual (infraestructura real) haciendo pull de nuestra SSOT. Cuando detecta diferencias, aplica los cambios necesarios para sincronizarlos.

Las principales ventajas de este enfoque son:
- Funciona en ambos sentidos: aplica los cambios hechos en el respositorio, pero si alguien cambia algo manualmente, el operador lo revierte.
- No requiere credenciales externas en el sistema de CI/CD.
- Mantiene una **reconciliación continua** entre el repositorio y el entorno. No es necesario esperar a que se ejecute un pipeline.

La principal desventaja es su complejidad: El Operador es introducido por herramientas como lo son [Argo CD](https://argo-cd.readthedocs.io/en/stable/) o [Flux](https://fluxcd.io/). Adicionalmente, es probable que se necesite un equipo específicamente para que mantenga la nueva estructura.

## Demostración práctica

En esta [documentación](../README.md#herramientas-necesarias) se encuentran las herramientas necesarias y un apartado de "paso a paso" para poner en práctica estos conceptos.

## Fuentes y lecturas asociadas

[gitiops.tech](https://www.gitops.tech/)

[What is GitOps, How GitOps works and Why it's so useful](https://www.youtube.com/watch?v=f5EpcWp0THw)

[GitLab](https://about.gitlab.com/es/topics/gitops/)