# Índice de Guías Rápidas por Lenguaje (Secure Coding Cheatsheets)

## 1. Propósito

Servir como referencia rápida de consulta técnica por stack tecnológico para los equipos de desarrollo y para el área de DevSecOps durante las revisiones de código. Consolida las referencias oficiales y destaca las funciones o patrones de alto riesgo específicos de cada lenguaje.

---

## 2. Referencias Base Universales

Para cualquier duda sobre implementación segura que no esté cubierta en las notas internas, la referencia oficial obligatoria de la organización es la serie de guías de **OWASP Cheat Sheet Series**:

* **Índice General:** [OWASP Cheat Sheet Series](https://owasp.org)
* **API Security:** [REST Security Cheat Sheet](https://owasp.orgcheatsheets/REST_Security_Cheat_Sheet.html)

---

## 3. Guía Rápida por Ecosistema (Banderas Rojas en Revisión Manual)

### JavaScript / TypeScript (Node.js & Frontend)

* **Referencia Oficial:** [Node.js Security Cheat Sheet](https://owasp.orgcheatsheets/Nodejs_Security_Cheat_Sheet.html)
* **Qué buscar en el código (Red Flags):**
  * **Ejecución dinámica / Inyección:** Uso de `eval()`, `new Function()`, `setTimeout(string)` o `child_process.exec()` con entradas del usuario (preferir `execFile()` o `spawn()` sin shell).
  * **XSS en Frontend:** Uso de `dangerouslySetInnerHTML` (React), `v-html` (Vue) o `innerHTML` sin sanitizar previamente con librerías como `DOMPurify`.
  * **Aleatoriedad insegura:** Uso de `Math.random()` para generar tokens, contraseñas o IDs de sesión (exigir `crypto.randomBytes()` o `crypto.randomUUID()`).
  * **Prototype Pollution:** Operaciones recursivas de fusión de objetos (`merge`, `clone`) sobre payloads JSON sin validar claves como `__proto__` o `constructor`.

### Python

* **Referencia Oficial:** [Python Security - OWASP](https://owasp.orgindex.html)
* **Qué buscar en el código (Red Flags):**
  * **Deserialización insegura:** Uso de `pickle.loads()`, `yaml.load()` sin especificar `Loader=yaml.SafeLoader`, o `eval()` / `exec()`.
  * **Inyección de comandos:** Uso de `subprocess.Popen(..., shell=True)` o `os.system()` con variables concatenadas.
  * **Inyección SQL en ORM:** Uso de `.raw()`, `.extra()` (en Django) o `text()` (en SQLAlchemy) concatenando f-strings (`f"SELECT * FROM users WHERE id = {user_id}"`).
  * **Aleatoriedad insegura:** Uso del módulo `random` para seguridad (exigir el módulo estándar `secrets`).

### Java / Kotlin (Spring Boot)

* **Referencia Oficial:** [Java Security Cheat Sheet](https://owasp.orgcheatsheets/Java_Security_Cheat_Sheet.html)
* **Qué buscar en el código (Red Flags):**
  * **Deserialización nativa:** Uso de `ObjectInputStream.readObject()` sobre datos no confiables.
  * **XXE (XML External Entity):** Instancias de `DocumentBuilderFactory` o `SAXParserFactory` sin deshabilitar explícitamente las declaraciones `DOCTYPE` y entidades externas.
  * **Inyección SQL:** Construcción de consultas JPQL o JDBC usando concatenación de strings en lugar de parámetros nombrados (`:param`) o `?`.
  * **Exposición de información:** Endpoints de Spring Boot Actuator (`/actuator/env`, `/actuator/heapdump`) expuestos públicamente sin autenticación.

### C# / .NET

* **Referencia Oficial:** [DotNet Security Cheat Sheet](https://owasp.orgcheatsheets/DotNet_Security_Cheat_Sheet.html)
* **Qué buscar en el código (Red Flags):**
  * **Deserialización insegura:** Uso de `BinaryFormatter`, `SoapFormatter` o `TypeNameHandling.All` en Newtonsoft.Json.
  * **Inyección SQL en Entity Framework:** Uso de `FromSqlRaw()` con interpolación de cadenas (`$"..."`) en lugar de `FromSqlInterpolated()` o parámetros `SqlParameter`.
  * **Validación de certificados desactivada:** Presencia de `ServerCertificateCustomValidationCallback` retornando siempre `true` en clientes HTTP.

### Go (Golang)

* **Referencia Oficial:** [Go Secure Coding Practices](https://owasp.orgindex.html)
* **Qué buscar en el código (Red Flags):**
  * **Plantillas HTML inseguras:** Uso de `text/template` en lugar de `html/template` para renderizar respuestas web (no escapa XSS automáticamente), o uso de tipos como `template.HTML` con entradas de usuario.
  * **TLS inseguro:** Configuración `InsecureSkipVerify: true` en `tls.Config` dentro de clientes HTTP.
  * **Aleatoriedad insegura:** Importación de `math/rand` en lugar de `crypto/rand` para generación de tokens o secretos.

---

## 4. Mantenimiento de este Documento

* **Regla de actualización:** Cada vez que se detecte una vulnerabilidad recurrente en un lenguaje específico durante una revisión manual o un pentest, se debe agregar la función o patrón inseguro a la lista de *Red Flags* correspondiente y, de ser posible, crear una regla automática en el analizador estático (SAST) para detectarlo en el futuro.

<!-- 

Cómo sacarle el máximo provecho

• Conviértelo en reglas automáticas: Fíjate que casi todas las Red Flags listadas arriba son nombres de funciones específicas (eval, pickle.loads, BinaryFormatter, InsecureSkipVerify). Puedes usar esa misma lista como base para configurar reglas en herramientas como Semgrep y detectarlas automáticamente antes de que el PR llegue a tu bandeja.
• Personalízalo: Si tu empresa usa algún otro lenguaje (como PHP, Ruby o Rust), simplemente agrega un bloque con el mismo formato de 4 puntos críticos.
 -->