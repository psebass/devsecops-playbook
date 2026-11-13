# Manual Operativo de Seguridad en el Desarrollo (DevSecOps Playbook)

Guía jerárquica de políticas, estándares y herramientas prácticas para asegurar aplicaciones multilenguaje desde el diseño hasta producción.

## Resumen del ecosistema documental completo

Pirámide operativa para gestionar múltiples lenguajes sin saturación:

1. Política de Clasificación: Filtra qué proyectos y cambios merecen tu tiempo.
2. Plantilla de PR: Actúa como aduana diaria donde el desarrollador declara el riesgo.
3. Estándar de Arquitectura: Define las reglas del juego antes de programar.
4. Checklist Agnóstico: Lupa operativa para revisar lógica y flujos sin importar el lenguaje.
5. Guía Rápida por Lenguaje: Diccionario rápido de red flags técnicas por tecnología.
6. Registro de Excepciones: Escudo de gobernanza cuando el negocio necesita salir rápido.

## Base estratégica de acciones

1. Política de Clasificación de Riesgos (Nivel Estratégico)

    • Propósito: Definir qué aplicaciones y qué cambios requieren tu intervención manual y cuáles no. Sin este documento, intentarás revisar todo y te saturarás.
    • Contenido clave: Categorización de aplicaciones por criticidad (Alta, Media, Baja) según los datos que manejan y su exposición (internet vs. intranet). Define disparadores obligatorios de revisión manual (ej. cambios en autenticación, pagos, infraestructura o manejo de archivos).
2. Plantilla Universal de Pull Request (PULL_REQUEST_TEMPLATE.md) (Nivel Flujo de Trabajo)
3. Estándar de Arquitectura Segura / Líneas Base (Nivel Diseño)

    • Propósito: Establecer las reglas de juego antes de que los desarrolladores escriban una sola línea de código, sin importar el lenguaje que usen.
    • Contenido clave: Definiciones agnósticas sobre cómo debe implementarse la autenticación (OAuth2/OIDC), el manejo de secretos (bóvedas como Vault o AWS Secrets Manager), el cifrado en tránsito y en reposo, y el formato estándar de logs de auditoría.

    • Propósito: Actuar como tu aduana diaria. Obliga al desarrollador a declarar el impacto de seguridad de su cambio antes de que tú abras el código.
    • Contenido clave: Preguntas cerradas de triaje (ej. "¿Se exponen nuevos endpoints?", "¿Se modifican permisos o roles?", "¿Se agregan dependencias externas?"). Según las respuestas, sabes en 30 segundos si el PR requiere una revisión profunda de tu parte.

4. Checklist de Revisión Segura de Código Agnóstico (Nivel Operativo)

    • Propósito: Ser tu guía rápida durante la lectura del código para no depender de la memoria ni distraerte con la sintaxis propia de cada lenguaje.
    • Contenido clave: Puntos de control universales basados en OWASP ASVS (Application Security Verification Standard), enfocados en validación de entradas, control de acceso (IDOR), manejo de errores y lógica de negocio.

5. Guías Rápidas por Lenguaje / Stack (Secure Coding Cheatsheets) (Nivel Táctico)

    • Propósito: Servir como material de consulta puntual tanto para ti como para los desarrolladores cuando surge una duda específica de una tecnología.
    • Contenido clave: En lugar de escribirlas desde cero, mantén un índice curado hacia las OWASP Cheat Sheet Series correspondientes a tus stacks (ej. Node.js, Java, Python, .NET), agregando únicamente las excepciones o librerías internas que usa tu empresa.

6. Registro de Excepciones y Deuda Técnica de Seguridad (Nivel Gobernanza)

    • Propósito: Documentar formalmente cuando un código pasa a producción con un riesgo aceptado por falta de tiempo, transfiriendo la responsabilidad al dueño del producto.
    • Contenido clave: Descripción del hallazgo, riesgo asociado, quién aprueba la salida a producción, plan de mitigación y fecha límite de corrección.

---
