# Manual Operativo de Seguridad en el Desarrollo (DevSecOps Playbook)

> **Guía jerárquica de políticas, estándares de arquitectura y controles operativos para asegurar aplicaciones multilenguaje desde el diseño hasta producción bajo un enfoque *Shift Left*.**

---

## Propósito

Este repositorio centraliza el marco de trabajo de seguridad aplicativa de la organización. Su objetivo es integrar la seguridad en las etapas más tempranas del ciclo de vida de desarrollo de software (SDLC), estandarizar los criterios de diseño sin importar el lenguaje de programación y agilizar las revisiones manuales de código mediante un enfoque basado en riesgos.

---

## Estructura Jerárquica de Documentos

Los documentos están organizados de mayor a menor nivel de abstracción (desde la estrategia y el diseño hasta la operación diaria en el código):

| Orden | Documento | Nivel | Propósito Principal |
| :--- | :--- | :--- | :--- |
| **01** | [Política de Clasificación de Riesgos](./docs/01-politica-clasificacion-riesgos.md) | Estratégico | Categoriza las aplicaciones por criticidad y define los disparadores obligatorios para revisiones manuales. |
| **02** | [Estándar de Arquitectura Segura](./docs/02-estandar-arquitectura-segura.md) | Diseño | Establece las líneas base obligatorias (autenticación, cifrado, secretos, APIs) antes de escribir código. |
| **03** | [Plantilla Universal de Pull Request](./.github/PULL_REQUEST_TEMPLATE.md) | Flujo de Trabajo | Actúa como aduana diaria donde el desarrollador declara el impacto de seguridad de sus cambios. |
| **04** | [Checklist de Revisión Agnóstico](./docs/04-checklist-revision-agnostico.md) | Operativo | Guía de inspección manual enfocada en lógica de negocio y flujos de datos, independiente del lenguaje. |
| **05** | [Índice de Guías por Lenguaje](./docs/05-guias-rapidas-lenguajes.md) | Táctico | Directorio de *red flags* y patrones peligrosos específicos por stack (JS, Python, Java, .NET, Go). |
| **06** | [Registro de Excepciones y Deuda Técnica](./docs/06-registro-excepciones-riesgo.md) | Gobernanza | Formato oficial para documentar, mitigar temporalmente y asignar responsabilidad ante riesgos aceptados. |

---

## Guía Rápida de Uso por Rol

* **Si eres Desarrollador:**
  1. Antes de iniciar un módulo nuevo, consulta el **Estándar de Arquitectura (02)**.
  2. Durante la programación, ten a mano las *red flags* de tu stack en las **Guías por Lenguaje (05)**.
  3. Al abrir un PR, completa con rigor el triaje de la **Plantilla Universal (03)**.

* **Si eres Tech Lead / Arquitecto:**
  1. Asegura que tu aplicación tenga asignada su criticidad según la **Política de Riesgos (01)**.
  2. Valida que el diseño técnico cumpla con las líneas base del **Estándar de Arquitectura (02)**.
  3. Gestiona las aprobaciones temporales mediante el **Registro de Excepciones (06)** cuando exista urgencia de negocio.

* **Si eres Ingeniero DevSecOps:**
  1. Utiliza la **Política de Riesgos (01)** y la **Plantilla de PR (03)** para filtrar qué cambios requieren tu auditoría manual.
  2. Ejecuta tus revisiones apoyándote en el **Checklist Agnóstico (04)** y el **Índice por Lenguaje (05)**.
  3. Mantén actualizada la bitácora del **Registro de Excepciones (06)** para dar seguimiento a los vencimientos.

---

## Estructura del Repositorio

Para que todos los enlaces de este `README.md` funcionen automáticamente, se propone la siguiente organización:

```text
devsecops-playbook/
├── .github/
│   └── PULL_REQUEST_TEMPLATE.md      # Documento 03
├── docs/
│   ├── 00-base.md
│   ├── 01-politica-clasificacion-riesgos.md
│   ├── 02-estandar-arquitectura-segura.md
│   ├── 04-checklist-revision-agnostico.md
│   ├── 05-guias-rapidas-lenguajes.md
│   └── 06-registro-excepciones-riesgo.md
└── README.md
```

---

## Referencias y Estándares Consultados

Los controles, criterios de revisión y directivas *Shift Left* de este manual están alineados con los siguientes marcos internacionales de referencia en seguridad aplicativa y DevSecOps:

* **Fundamentos DevSecOps y Shift Left:**
  * [IBM — ¿Qué es la seguridad Shift Left?](https://ibm.com) — Principios de integración temprana de pruebas y seguridad en el ciclo de desarrollo.
  * [Microsoft Learn — Disciplina de adopción de seguridad en el desarrollo (DevSecOps)](https://microsoft.com) — Gobernanza, puertas de calidad (*Quality Gates*) y responsabilidad compartida.
  * [NIST SP 800-218 — Secure Software Development Framework (SSDF)](https://nist.gov) — Marco estándar para integrar prácticas de desarrollo seguro en cualquier modelo de SDLC.

* **Estándares de Verificación y Revisión de Código:**
  * [OWASP ASVS (Application Security Verification Standard)](https://owasp.org) — Base técnica para los requisitos de arquitectura y los puntos de control del checklist agnóstico.
  * [OWASP Cheat Sheet Series](https://owasp.org) — Guías tácticas de codificación segura por tecnología y prevención de vulnerabilidades específicas.
  * [OWASP Top 10](https://owasp.org) — Referencia de los riesgos más críticos en aplicaciones web y APIs.
