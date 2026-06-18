# VidalCasino 2.0 - Casino Backend

Este repositorio contiene el backend principal del proyecto **VidalCasino 2.0**, desarrollado para la evaluación EP3 de Introducción a Herramientas DevOps. Este servicio actúa como API central del sistema de casino y se comunica con la base de datos PostgreSQL y con otros componentes internos desplegados en Amazon EKS.

## Descripción general

`casino-backend` gestiona funcionalidades principales del sistema, como autenticación, usuarios, transacciones y operaciones base del casino. Dentro de la arquitectura del proyecto, este servicio se mantiene como un componente interno del clúster Kubernetes, expuesto mediante un Service de tipo `ClusterIP`.

El acceso externo al sistema se realiza únicamente a través del frontend, mientras que el backend permanece protegido dentro del clúster.

## Arquitectura del sistema

El sistema VidalCasino está compuesto por:

- **casino-frontend:** interfaz web expuesta mediante LoadBalancer.
- **casino-backend:** backend principal interno.
- **bonos-service:** microservicio para gestión de bonos.
- **apuestas-service:** microservicio para apuestas deportivas.
- **estadisticas-service:** microservicio para estadísticas.
- **postgres:** base de datos interna del sistema.

## Tecnologías utilizadas

- Node.js
- Express
- Docker
- Kubernetes
- Amazon EKS
- Amazon ECR
- GitHub Actions
- PostgreSQL
- AWS Academy Learner Lab

## Despliegue en Kubernetes

Los manifiestos Kubernetes del backend se encuentran en:

```txt
k8s/

Archivos principales:

k8s/deployment.yaml
k8s/service.yaml

El backend se despliega como un Deployment y se expone internamente mediante un Service de tipo ClusterIP en el puerto 3000.

CI/CD

El despliegue automático se realiza mediante GitHub Actions. El workflow se encuentra en:

.github/workflows/deploy.yml

El pipeline se ejecuta al realizar un push sobre la rama deploy.

El flujo CI/CD realiza:

Descarga del código del repositorio.
Configuración de credenciales temporales de AWS Academy.
Inicio de sesión en Amazon ECR.
Construcción de la imagen Docker.
Publicación de la imagen en Amazon ECR con tags latest, v1.0.1 y SHA del commit.
Conexión de kubectl al clúster EKS.
Actualización del Deployment en Kubernetes.
Verificación del rollout.
Comandos de verificación
kubectl get deployment casino-backend
kubectl get svc casino-backend
kubectl get pods -l app=casino-backend -o wide
kubectl describe deployment casino-backend
Estado esperado
Deployment disponible.
Service interno tipo ClusterIP.
Pod en estado Running.
Imagen almacenada en Amazon ECR.
Pipeline CI/CD exitoso en GitHub Actions.