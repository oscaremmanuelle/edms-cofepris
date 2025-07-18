# URS – eDMS COFEPRIS

## 1. Introducción
Este documento describe los **requisitos** para el desarrollo de un **sistema electrónico de gestión documental** (eDMS) que permita digitalizar y controlar todos los documentos regulados por COFEPRIS en ATRAMAT.  
Su finalidad es garantizar la integridad, disponibilidad y trazabilidad de los registros, facilitar auditorías regulatorias y modernizar el manejo de **miles de documentos** que hoy se gestionan en papel.

## 2. Alcance
El sistema cubrirá los siguientes ámbitos y documentos:

- **Procesos**:  
  - Creación y revisión de Procedimientos Normalizados de Operación (PNO).  
  - Registro y liberación de lotes de fabricación.  
  - Gestión de expedientes de registro sanitario.  
- **Familias de documentos**:  
  - Manual de Calidad, PNOs, Especificaciones, Órdenes Maestras, Registros de Lote, Expedientes del Dispositivo, Certificados de BPF/ISO.  
- **Usuarios**:  
  - Responsable Sanitario, Gestión de Calidad, Producción, TI, Auditores COFEPRIS.
- **Interfaces**:  
  - Web (navegador), posibles integraciones con DIGIPRiS/ERP.

## 3. Requisitos Funcionales Críticos”

	1.	Control de Versiones
	•	Creación de nueva versión de un documento.
	•	Aprobación de versión (solo usuarios con rol “Aprobador”).
	•	Obsolescencia y retiro de versiones antiguas.
	2.	Workflows de Aprobación
	•	Definir rutas (e.g. Autor → QA → Responsable Sanitario).
	•	Notificaciones automáticas por correo o en UI.
	•	Visualización del estado (Borrador, En Revisión, Aprobado, Obsoleto).
	3.	Firma Electrónica
	•	Integración con certificado X.509 o proveedor (DocuSign/Adobe).
	•	Registro de firma con sello de tiempo.
	•	Equivalencia legal según FDA 21 CFR Part 11.
	4.	Gestión de Metadatos
	•	Campos obligatorios: ID único, título, familia normativa, producto asociado, versión, autor, fecha.
	•	Campos opcionales: palabras clave, idioma, retención (fecha de expiración).
	5.	Búsqueda y Recuperación
	•	Búsqueda por metadatos.
	•	Búsqueda full-text sobre OCR.
	•	Filtrado por estado, fecha, família normativa.
	6.	Exportación de Paquetes Regulatorios
	•	Generar ZIP/PDF/A con estructura RIS Art. 179/180.
	•	Incluir checklists automáticos de cumplimiento (p.ej. control de cambios).
	7.	Control de Retención y Disposición
	•	Alarmas para documentos próximos a expirar.
	•	Flujo para marcar “Descartar papel” o “Conservar hasta…”.

## 4. Requisitos No Funcionales

1. **Seguridad**  
   - Autenticación y autorización basada en roles (RBAC).  
   - Cifrado de datos **at-rest** (AES-256) y **in-transit** (TLS 1.2+).  
   - Gestión de contraseñas seguras (longitud mínima 12, expiración configurable).

2. **Rendimiento**  
   - Soportar la ingestión simultánea de **100 documentos/hora** sin degradar respuesta.  
   - Tiempos de respuesta de búsqueda < 2 s para queries por metadatos.

3. **Disponibilidad y Resiliencia**  
   - SLA ≥ 99.5 % uptime.  
   - Diseño con alta disponibilidad (mín. 2 réplicas de backend y de base de datos).

4. **Escalabilidad**  
   - Arquitectura capaz de escalar horizontalmente (stateless API + almacenamiento S3).  
   - Soportar crecimiento de hasta **500,000 documentos** en el primer año.

5. **Usabilidad**  
   - Interfaz web responsiva, compatible con Chrome, Edge y Safari.  
   - Navegación intuitiva: carga/descarga, aprobación con un máximo de 3 clics.

6. **Mantenibilidad**  
   - Código documentado y modularizado (tests unitarios ≥ 80 % cobertura).  
   - CI/CD automatizado para despliegues y pruebas.

7. **Cumplimiento y Auditoría**  
   - Audit-trail inmutable de todas las acciones (create/read/update/delete).  
   - Generación de reportes de auditoría en PDF.

8. **Backup y Recuperación**  
   - Backups diarios de base de datos y bucket de archivos, retención de 30 días.  
   - Procedimiento documentado de restore en < 2 horas.