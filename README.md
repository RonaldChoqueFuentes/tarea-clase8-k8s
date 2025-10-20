
# TAREA 8 
### Curso: Docker && Kubernate
### Estudiante: Ronald Choque Fuentes

**a) Descripción del proyecto:**
- Stack desplegado (frontend + backend)
- Conceptos aplicados (Ingress, health probes, HPA)

**b) Instrucciones de despliegue:**

## Parte 1: Desplegar Frontend 
### 1.1 Crear Deployment con health probes

```bash
kubectl apply -f k8s/frontend-deployment.yaml
```
![alt text](/screenshots/image.png)

### 1.2 Crear Service (ClusterIP)

```bash
kubectl apply -f k8s/frontend-service.yaml
```
![alt text](/screenshots/image-1.png)

## Parte 2: Desplegar Backend

### 2.1 Crear Deployment con health probes y resources
```bash
kubectl apply -f k8s/backend-deployment.yaml
```
![alt text](/screenshots/image-2.png)

### 2.2 Crear Service (ClusterIP)
```bash
kubectl apply -f k8s/backend-service.yaml
```
![alt text](/screenshots/image-3.png)

## Parte 3: Configurar Ingress
### 3.1 Habilitar NGINX Ingress Controller
```bash
minikube addons enable ingress
```
![alt text](/screenshots/image-4.png)
### 3.2 Crear Ingress
```bash
kubectl apply -f k8s/ingress.yaml
```
![alt text](/screenshots/image-5.png)

### 3.3 Probar Ingress

```bash
kubectl get ingress app-ingress
curl http://$(kubectl get ingress app-ingress -o jsonpath='{.status.loadBalancer.ingress[0].ip}')/
curl http://$(kubectl get ingress app-ingress -o jsonpath='{.status.loadBalancer.ingress[0].ip}')/api
```

![alt text](/screenshots/image-6.png)

## Parte 4: Configurar HPA

### 4.1 Habilitar Metrics Server

```bash
minikube addons enable metrics-server
```
![alt text](/screenshots/image-7.png)

### 4.2 Crear HPA para backend

```bash
kubectl apply -f k8s/hpa.yaml
```
![alt text](/screenshots/image-9.png)

### 4.3 Generar carga

```bash
kubectl run load-generator --image=busybox:1.28 --rm -it --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://backend-service; done"
```
![alt text](/screenshots/image-10.png)

### 4.4 Observar escalado
```bash
# Terminal 1
kubectl get hpa backend-hpa --watch

# Terminal 2
watch kubectl get pods -l app=backend
```

![alt text](/screenshots/image-11.png)

**c) Comandos de verificación:**
```bash
kubectl get all
kubectl get ingress
kubectl get hpa
kubectl top pods
```

**d) Capturas de pantalla:**
1. Ingress funcionando (curl a `/` y `/api`)
2. Health probes configurados (`kubectl describe pod`)
3. HPA en reposo (TARGETS 0%/50%)
4. HPA escalando bajo carga (TARGETS >50%)
5. Pods escalados (de 2 a 4-5)

**e) Comandos de limpieza:**
```bash
kubectl delete ingress app-ingress
kubectl delete hpa backend-hpa
kubectl delete service frontend-service backend-service
kubectl delete deployment frontend backend
```

---