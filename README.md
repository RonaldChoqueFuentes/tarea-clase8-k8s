
# TAREA 8 
### Curso: Docker && Kubernate
### Estudiante: Ronald Choque Fuentes

**a) Descripción del proyecto:**
- Stack desplegado (frontend + backend)
- Conceptos aplicados (Ingress, health probes, HPA)

**b) Instrucciones de despliegue:**
1. Habilitar addons (ingress, metrics-server)
2. Aplicar manifests
3. Verificar recursos
4. Probar Ingress
5. Probar HPA con carga

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