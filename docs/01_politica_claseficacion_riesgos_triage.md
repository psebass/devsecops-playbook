# Política de Clasificación de Riesgos y Revisiones de Seguridad (DevSecOps)

## 1. Objetivo

Establecer criterios objetivos para categorizar las aplicaciones y los cambios de código según su nivel de riesgo. Esto permite priorizar las revisiones manuales de seguridad en los componentes críticos, optimizando los tiempos de entrega sin comprometer la postura de seguridad.

## 2. Clasificación de Aplicaciones (Nivel Macro)

Toda aplicación bajo gestión debe clasificarse en uno de los siguientes tres niveles antes de su primer pase a producción:

* **Nivel 1 - Crítica (Riesgo Alto):**
  * **Criterios:** Expuesta directamente a internet, procesa datos personales sensibles (PII), información financiera, credenciales, o es vital para la continuidad operativa del negocio.
  * **Requisito de revisión:** Requiere validación de arquitectura previa y revisión manual de seguridad obligatoria en cambios críticos.

* **Nivel 2 - Importante (Riesgo Medio):**
  * **Criterios:** Uso interno (intranet/VPN) con acceso a datos corporativos confidenciales, o aplicaciones públicas meramente informativas sin manejo de sesiones de usuario.
  * **Requisito de revisión:** Controles automatizados obligatorios y revisión manual muestral o ante cambios estructurales.

* **Nivel 3 - Estándar (Riesgo Bajo):**
  * **Criterios:** Herramientas internas de soporte, entornos de prueba aislados o aplicaciones que no procesan ni almacenan información sensible.
  * **Requisito de revisión:** Validación exclusiva mediante herramientas automatizadas en el pipeline (SAST/Secret Scanning) y revisión por pares del equipo de desarrollo.

## 3. Disparadores de Revisión Manual Obligatoria (Nivel Micro / Pull Request)

Independientemente del nivel de la aplicación (especialmente en Nivel 1 y Nivel 2), **se debe solicitar la revisión manual del equipo DevSecOps** siempre que un cambio incluya al menos uno de los siguientes disparadores:

1. **Autenticación y Sesiones:** Modificaciones en flujos de login, registro, recuperación de contraseñas, manejo de tokens (JWT, OAuth) o gestión de sesiones.
2. **Control de Acceso:** Creación o modificación de roles, permisos, políticas de autorización o filtros de acceso a recursos.
3. **Exposición de Superficie:** Creación de nuevos endpoints públicos, APIs externas o cambios en configuraciones de CORS y cabeceras de seguridad.
4. **Manejo de Archivos y Datos Críticos:** Implementación de carga/descarga de archivos, procesamiento de pagos, operaciones criptográficas o consultas SQL nativas (sin ORM).
5. **Infraestructura y Dependencias:** Cambios en archivos de configuración de contenedores (Dockerfile), pipelines de despliegue (CI/CD), permisos cloud (IAM) o incorporación de nuevas librerías de terceros.

## 4. Acuerdos de Nivel de Servicio (SLA) para Revisiones

* **Cambios Estándar (sin disparadores):** Aprobación automática tras pasar los controles del pipeline y la revisión funcional del equipo.
* **Cambios Críticos (con disparadores):** El equipo de desarrollo debe etiquetar al área de seguridad con al menos **24 a 48 horas de antelación** al despliegue planificado.
* **Emergencias / Hotfixes:** Se permite el despliegue inmediato con aprobación del líder técnico, sujeto a una auditoría manual obligatoria dentro de las **24 horas posteriores** al incidente.
