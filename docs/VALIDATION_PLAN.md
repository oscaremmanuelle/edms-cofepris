# Plan de Validación IQ/OQ/PQ
Este documento describe el protocolo de validación del sistema eDMS conforme a los requisitos regulatorios (NOM-241, FDA 21 CFR Part 11, ISO 13485).
## 1. Alcance de la Validación
- Instalación (IQ) de componentes de infraestructura.
- Operacional (OQ) de funcionalidades críticas.
- Performance (PQ) bajo condiciones reales de uso.

## 2. Roles y Responsables
| Actividad            | Responsable           | Soporte      |
|----------------------|-----------------------|--------------|
| Preparación IQ       | TI / DevOps           | QA           |
| Ejecución IQ         | TI / DevOps           | QA           |
| Ejecución OQ         | QA / Responsable Sanitario | TI       |
| Ejecución PQ         | QA / Responsable Sanitario | TI       |
| Revisión de Protocolos | Responsable Sanitario | QA / TI     |

## 3. IQ – Installation Qualification
### 3.1 Objetivo
Verificar instalación correcta de hardware, software y servicios.
### 3.2 Requisitos Previos
- Servidor con OS **Ubuntu 22.04 LTS** (Linux estable y muy usado en producción).  
- Base de datos **PostgreSQL 14**.  
- **MinIO** (local) o **AWS S3** (región us-east-1) configurado.  
- Dominio: **docs.tuempresa.com** con certificados **Let’s Encrypt** (TLS 1.2+).
### 3.3 Procedimiento IQ
1. Verificar versión de OS:
   - Comando: `uname -a`
2. Verificar servicios:
   - `docker ps` debe mostrar db, minio, backend, frontend.
3. Comprobación de configuración de red:
   - Ping al dominio, puertos abiertos.
4. Verificar acceso SSH/DB:
   - Conexión a Postgres: `psql -h localhost -U edms edms
5. Documentar resultados (ok/nok) en la tabla de evidencias.

## 4. OQ – Operational Qualification
### 4.1 Objetivo
Validar funcionalidades críticas según URS.
### 4.2 Casos de Prueba
| ID  | Funcionalidad            | Entrada de Prueba                | Resultado Esperado                | Estado (OK/NOK) | Evidencia          |
|-----|--------------------------|----------------------------------|-----------------------------------|-----------------|--------------------|
| OQ-1| Ingesta de documento     | PDF de ejemplo                   | Registro creado con metadatos     |                 | captura / log      |
| OQ-2| Control de versiones     | Crear nueva versión              | Versión disponible y aprobable    |                 | captura UI         |
| OQ-3| Workflow aprobación      | Documento en borrador            | Flujo Autor→QA→Sanitario completo |                 | notificaciones     |
| OQ-4| Firma electrónica        | Documento aprobado               | Firma registrada con sello        |                 | documento firmado  |
| OQ-5| Búsqueda                 | Búsqueda por metadata            | Resultado en <2 s                 |                 | tiempos de respuesta|

## 5. PQ – Performance Qualification
### 5.1 Objetivo
Verificar rendimiento y estabilidad bajo carga.
### 5.2 Escenarios de Prueba
| ID   | Escenario                   | Descripción                                    | Métrica Esperada            | Estado (OK/NOK) | Evidencia         |
|------|-----------------------------|------------------------------------------------|-----------------------------|-----------------|-------------------|
| PQ-1 | Ingesta masiva              | 100 documentos/hora                             | CPU <70%, tasa completa     |                 | gráficas Prometheus|
| PQ-2 | Búsqueda concurrente        | 50 usuarios simultáneos haciendo búsquedas      | Latencia <2 s               |                 | logs Locust      |
| PQ-3 | Backup y Restore            | Restaurar backup completo                      | <2 horas                    |                 | informe de restore|
| PQ-4 | Failover                    | Simular caída de nodo backend                  | Sistema sin interrupción    |                 | logs HA          |

## 6. Cronograma de Validación
## 6. Cronograma de Validación
| Fase | Duración Estimada | Fecha Inicio   | Fecha Fin      |
|------|-------------------|----------------|----------------|
| IQ   | 2 semanas         | 01/09/2025     | 14/09/2025     |
| OQ   | 4 semanas         | 15/09/2025     | 12/10/2025     |
| PQ   | 4 semanas         | 13/10/2025     | 09/11/2025     |

## 7. Aprobación
| Fase | Firma Responsable            | Fecha       |
|------|------------------------------|-------------|
| IQ   | ____________________________ |             |
| OQ   | ____________________________ |             |
| PQ   | ____________________________ |             |
