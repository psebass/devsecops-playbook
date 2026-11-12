# Estándar de Arquitectura Segura y Líneas Base (DevSecOps)

## 1. Propósito y Alcance

Definir los controles de seguridad obligatorios a nivel de diseño y arquitectura para todas las aplicaciones desarrolladas o mantenidas por la organización, independientemente del lenguaje de programación o framework utilizado.

---

## 2. Identidad, Autenticación y Sesiones

* **Protocolos Estándar:** Toda autenticación de usuarios debe delegarse a un proveedor de identidad centralizado (IdP) utilizando estándares abiertos como **OAuth 2.0 / OpenID Connect (OIDC)** o **SAML 2.0**. Queda prohibido implementar sistemas de autenticación propios desde cero.
* **Autenticación Multifactor (MFA):** Obligatoria para todos los accesos administrativos, entornos productivos y aplicaciones clasificadas como Nivel 1 (Críticas).
* **Gestión de Tokens (JWT / Sesiones):**
  * Los tokens de acceso (*Access Tokens*) deben tener una vida útil corta (máximo **15 a 60 minutos**).
  * Los *Refresh Tokens* deben rotarse en cada uso y poder revocarse de forma centralizada.
  * En aplicaciones web, los tokens de sesión deben almacenarse en cookies con los atributos `HttpOnly`, `Secure` y `SameSite=Strict` (o `Lax`), evitando el uso de `localStorage`.
* **Almacenamiento de Contraseñas (si aplica):** Deben almacenarse utilizando funciones de derivación de claves resistentes a fuerza bruta: **Argon2id** (preferido) o **bcrypt** (factor de costo mínimo 12).

---

## 3. Control de Acceso y Autorización

* **Denegación por Defecto:** Todo endpoint, recurso o función debe estar bloqueado por defecto. El acceso debe concederse de forma explícita mediante políticas definidas.
* **Validación en el Servidor:** Ninguna decisión de autorización puede depender de validaciones en el cliente (*frontend*). Cada petición debe validar los permisos en el *backend*.
* **Prevención de IDOR:** El acceso a cualquier objeto por identificador (ID, UUID) debe verificar en la capa de negocio o base de datos que el usuario autenticado sea el propietario o tenga permisos explícitos sobre ese registro específico.
* **Principio de Mínimo Privilegio:** Las cuentas de servicio, conexiones a bases de datos y roles en la nube (IAM) deben contar únicamente con los permisos estrictamente necesarios para operar (ej. una aplicación web nunca debe conectarse a la base de datos como `root` o `sa`).

---

## 4. Criptografía y Protección de Datos

* **Cifrado en Tránsito:** Toda comunicación externa e interna entre microservicios debe utilizar **TLS 1.2 o superior** (preferiblemente TLS 1.3). El tráfico HTTP sin cifrar debe redirigirse automáticamente a HTTPS.
* **Cifrado en Reposo:** Las bases de datos, volúmenes de almacenamiento, backups y buckets que contengan datos confidenciales o personales (PII) deben estar cifrados utilizando **AES-256**.
* **Gestión de Secretos:**
  * Queda estrictamente prohibido almacenar credenciales, API keys, certificados o cadenas de conexión en el código fuente, variables de entorno planas en repositorios o imágenes de contenedores.
  * Todos los secretos deben inyectarse en tiempo de ejecución desde un gestor centralizado (ej. AWS Secrets Manager, HashiCorp Vault, Azure Key Vault).

---

## 5. Diseño de APIs y Fronteras de Confianza

* **Validación de Esquemas:** Toda API debe validar estrictamente la estructura, tipo de dato, longitud y rango de las entradas mediante esquemas definidos (ej. OpenAPI/Swagger, JSON Schema) antes de procesar la lógica de negocio.
* **Limitación de Tasa (*Rate Limiting*):** Los endpoints públicos, especialmente los de autenticación y recuperación de cuentas, deben implementar controles de *rate limiting* para mitigar ataques de fuerza bruta y denegación de servicio (DoS).
* **Configuración de CORS:** Las políticas de *Cross-Origin Resource Sharing* deben especificar explícitamente los dominios autorizados. El uso de comodines (`Access-Control-Allow-Origin: *`) está prohibido en APIs autenticadas.

---

## 6. Registro, Monitoreo y Manejo de Errores

* **Logs de Auditoría Estructurados:** Las aplicaciones deben generar logs en formato estructurado (JSON) registrando eventos críticos: inicios de sesión (exitosos y fallidos), cambios de permisos, acceso a datos sensibles y errores de validación.
* **Sanitización de Logs:** Queda prohibido registrar información sensible en los logs (contraseñas, tokens, números de tarjeta, documentos de identidad completos).
* **Manejo Seguro de Excepciones:** Ante un error interno, la aplicación debe capturar la excepción, registrar el detalle completo de forma interna y devolver al usuario únicamente un mensaje genérico con un identificador de correlación (*Correlation ID*).

---

## 7. Seguridad en Contenedores e Infraestructura

* **Imágenes Mínimas y Sin Privilegios:** Los contenedores deben construirse sobre imágenes base oficiales y ligeras (ej. `distroless`, `alpine`) y ejecutarse siempre con un **usuario sin privilegios (`non-root`)**.
* **Inmutabilidad:** Los entornos productivos deben ser inmutables. No se permiten modificaciones manuales ni conexiones SSH directas a servidores o contenedores en ejecución para aplicar parches.

<!-- 

Cómo aplicar este documento en tu día a día

• Antes de iniciar un proyecto: Pide al líder técnico que revise este documento y confirme cómo cumplirá cada punto en su diagrama de arquitectura.
• Durante tus revisiones manuales de PR: Si ves algo que viola estas reglas (por ejemplo, un token guardado en localStorage o un CORS abierto con *), ya no tienes que debatir opiniones; simplemente citas la sección correspondiente de este estándar para exigir el cambio.

 -->