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

TBC