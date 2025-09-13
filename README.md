
# Actividad 3 – Integración de DevOps y DevSecOps con HTTP, DNS, TLS y 12-Factor App
## Parte teórica

### 1. Introducción a DevOps
- **Qué es y qué no es DevOps**:  
  - Es: cultura + prácticas + automatización + feedback continuo, del código a producción.  
  - No es: solo herramientas ni un rol aislado.  
- **Del código a producción vs Waterfall**:  
  - Waterfall: fases lineales, poca retroalimentación.  
  - DevOps: ciclos cortos, integración continua, “tú lo construyes, tú lo ejecutas”.  
- **Mitos y realidades**:  
  - Mito: “DevOps = Jenkins + Docker”.  
  - Realidad: CALMS, puertas de calidad (ejemplo: en Makefile → verificar lint, TLS >= 1.3, tests pasan).  

### 2. Marco CALMS en acción
**Culture**: colaboración para evitar silos.  
**Automation**: Makefile (ej. `make deps`, `make run`).  
**Lean**: idempotencia en HTTP, evitar desperdicio.  
**Measurement**: endpoints de salud, métricas de latencia con `curl -w`.  
**Sharing**: proponer runbooks/postmortems.  

Ejemplo de correspondencia CALMS ↔ archivos del laboratorio:
- Culture → `Instrucciones.md` (roles claros).  
- Automation → `Makefile`.  
- Lean → `app.py` sin estados locales.  
- Measurement → logs y `ss -lnt`.  
- Sharing → README + evidencias.  

### 3. Visión cultural y DevSecOps
 **Colaboración**: romper silos Dev/QA/Ops.  
 **Evolución a DevSecOps**: seguridad desde el inicio (TLS cabeceras, escaneo dependencias).  
 **Escenario retador**: fallo de certificado → rollback rápido, aprendizaje cultural (no culpas, mejora de proceso).  
 **Controles de seguridad (sin contenedores)**:  
  1. Nginx configurado con TLS ≥ 1.3 (archivo `nginx.conf`).  
  2. systemd con `User=` no privilegiado (archivo `.service`).  
  3. Escaneo de dependencias en CI/CD (Makefile → `make security-scan`).  

### 4. Metodología 12-Factor App
Factores elegidos:
1) Configuración por entorno (`PORT`, `MESSAGE` vía variables).  
2)) Binding de puertos (app escucha en `:8080`, Nginx redirige `:443`).  
3))) Logs como flujos (`journalctl`, redirigir a stdout)..
4)) Stateless processes (no guardar estado en `app.py`, depender de backing services).  


## Parte práctica

### 1. Automatización reproducible con Make y Bash

