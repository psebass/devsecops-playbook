# Registro de Excepciones y Deuda Técnica de Seguridad (DevSecOps)

## 1. Propósito

Establecer un proceso formal para gestionar y documentar los casos excepcionales en los que una aplicación requiere pasar a producción incumpliendo temporalmente un estándar de seguridad o manteniendo un hallazgo abierto. Garantiza la trazabilidad, la asignación de responsabilidad sobre el riesgo y un compromiso con fecha límite para su remediación.

---

## 2. Reglas de Gobernanza para Excepciones

* **Carácter Temporal:** Ninguna excepción de seguridad es permanente. Toda excepción debe tener una fecha de vencimiento máxima según la criticidad del hallazgo:
  * **Riesgo Crítico:** **No admite excepciones** (bloqueo obligatorio de despliegue hasta su mitigación).
  * **Riesgo Alto:** Máximo **15 a 30 días** calendario (requiere aprobación de Gerencia de Tecnología / CISO).
  * **Riesgo Medio:** Máximo **60 a 90 días** calendario (requiere aprobación del Product Owner y Líder Técnico).
  * **Riesgo Bajo:** Se registra directamente en el backlog técnico del equipo sin requerir excepción formal.
* **Controles Compensatorios:** Siempre que sea posible, debe aplicarse una medida temporal (ej. reglas en WAF, restricción por IP, monitoreo reforzado) mientras se desarrolla la solución definitiva.
* **Renovaciones:** Si al llegar la fecha de vencimiento el problema no ha sido resuelto, la excepción caduca automáticamente y el equipo debe gestionar una nueva firma de aceptación de riesgo ante la dirección.

---

## 3. Plantilla de Solicitud y Aceptación de Riesgo

*(Completar un registro por cada hallazgo o conjunto de hallazgos relacionados en un Pull Request / Despliegue).*

### Identificación del Registro

* **ID de Excepción:** `EXC-SEC-YYYY-001`
* **Fecha de Solicitud:** [DD/MM/AAAA]
* **Aplicación / Repositorio:** [Nombre de la App y URL del repo]
* **Nivel de Criticidad de la App:** [ ] Nivel 1 (Alta) | [ ] Nivel 2 (Media) | [ ] Nivel 3 (Baja)
* **Pull Request / Release asociado:** [Enlace al PR o versión]

### Detalle Técnico del Hallazgo

* **Descripción de la Vulnerabilidad / Incumplimiento:** 
  *(Ej. El endpoint `/api/v1/reportes` no implementa validación de paginación máxima ni rate limiting, incumpliendo la sección 5 del Estándar de Arquitectura Segura).*
* **Nivel de Riesgo Evaluado:** [ ] Alto | [ ] Medio | [ ] Bajo
* **Impacto Potencial:** 
  *(Ej. Posible degradación del servicio o consumo excesivo de memoria si un usuario autenticado solicita millones de registros).*

### Justificación y Plan de Remediación

* **Motivo de la Excepción:** 
  *(Ej. Compromiso contractual de entrega para cliente corporativo en fecha fija; la refactorización requiere 3 días adicionales).*
* **Control Compensatorio Aplicado (Temporal):** 
  *(Ej. Se configuró un límite de 30 peticiones por minuto en el API Gateway/WAF para esa ruta específica).*
* **Ticket de Remediación en Backlog:** [Enlace al ticket en Jira/Azure DevOps/GitHub Issues]
* **Fecha Límite de Corrección (Vencimiento):** [DD/MM/AAAA]

### Aprobación y Aceptación Formal del Riesgo

*Al firmar o aprobar este registro, el responsable del negocio/producto declara conocer el riesgo técnico expuesto por el área de DevSecOps y asume la responsabilidad operativa de liberar el cambio bajo estas condiciones.*

* **Evaluado por (Ingeniero DevSecOps):** [Nombre y Firma/Aprobación]
* **Responsable Técnico (Tech Lead / Arquitecto):** [Nombre y Firma/Aprobación]
* **Aprobado por (Product Owner / Gerente):** [Nombre y Firma/Aprobación]

---

## 4. Tabla de Seguimiento Centralizado (Bitácora)

*Mantener esta tabla actualizada en la wiki central para auditorías y seguimiento mensual.*

| ID Excepción | Aplicación | Riesgo | Fecha Solicitud | Fecha Vencimiento | Ticket Backlog | Responsable | Estado |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `EXC-SEC-2026-001` | Portal Clientes | Medio | 12/11/2026 | 12/01/2027 | `PROJ-412` | J. Pérez (PO) | 🟡 Abierta |
| `EXC-SEC-2026-002` | API Pagos | Alto | 05/10/2026 | 04/11/2026 | `PAG-108` | M. Gómez (TL) | 🟢 Remediada |

<!-- 
 -->