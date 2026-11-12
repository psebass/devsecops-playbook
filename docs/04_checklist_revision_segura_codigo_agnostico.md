# Checklist de Revisión Segura de Código Agnóstico (Nivel Operativo)

## 1. Propósito

Servir como guía rápida de inspección manual durante la revisión de *Pull Requests* en múltiples lenguajes de programación. Está basado en el estándar **OWASP ASVS** y enfocado en detectar fallos de lógica, control de acceso y flujo de datos que las herramientas automatizadas (SAST) no identifican.

---

## 2. Flujo de Entrada de Datos (Input Validation)

*Rastrea toda variable que provenga de una petición HTTP (body, query params, headers, cookies), una cola de mensajes o un archivo.*

* [ ] **Validación en frontera:** ¿Se valida el tipo de dato, longitud mínima/máxima y formato esperado antes de que el dato entre a la lógica de negocio?
* [ ] **Listas blancas (Allowlists):** En campos con valores predefinidos (ej. roles, estados, monedas, tipos de documento), ¿se valida contra una lista cerrada de valores permitidos en lugar de intentar bloquear valores malos?
* [ ] **Deserialización segura:** Si el código convierte texto (JSON, XML, YAML) a objetos internos, ¿utiliza librerías seguras y evita deserializar clases o tipos dinámicos enviados por el usuario?
* [ ] **Protección contra Mass Assignment:** Al crear o actualizar registros (ej. un perfil de usuario), ¿se filtran explícitamente los campos permitidos para evitar que el usuario inyecte propiedades ocultas como `isAdmin: true` o `role: "admin"`?

---

## 3. Control de Acceso y Autorización (Access Control)

*Verifica quién ejecuta la acción y sobre qué recurso lo hace.*

* [ ] **Verificación en cada endpoint:** ¿La nueva ruta o función tiene explícitamente asociado un middleware, decorador o validación de permisos?
* [ ] **Prevención de IDOR (Acceso a objetos por ID):** Cuando el código recibe un identificador (ej. `GET /pedidos/{id}` o `DELETE /documentos/{id}`), ¿la consulta a la base de datos filtra también por el ID del usuario autenticado (ej. `WHERE id = ? AND user_id = ?`)?
* [ ] **Elevación de privilegios:** ¿Un usuario con rol estándar puede invocar funciones administrativas simplemente conociendo la URL o cambiando el método HTTP (de `GET` a `POST`/`PUT`)?

---

## 4. Interacción con Bases de Datos y Sistemas Externos

*Revisa cómo el código construye consultas o comandos hacia otros sistemas.*

* [ ] **Consultas parametrizadas:** ¿Todas las consultas SQL o NoSQL usan parámetros vinculados (*Prepared Statements*)? *(Señal de alerta: concatenación de strings con `+`, f-strings o template literals dentro de una consulta).*
* [ ] **Ordenamiento y paginación seguros:** Si el usuario puede elegir el campo de ordenamiento (`ORDER BY`) o el límite de resultados, ¿se valida el nombre de la columna contra una lista blanca y se impone un límite máximo de registros (ej. máximo 100)?
* [ ] **Prevención de SSRF (Server-Side Request Forgery):** Si el código realiza peticiones HTTP hacia una URL proporcionada o influenciada por el usuario (ej. webhooks, descarga de avatares), ¿se valida que la URL no apunte a direcciones IP internas (`127.0.0.1`, `169.254.169.254`, redes privadas)?
* [ ] **Ejecución de comandos del sistema:** Si el código invoca comandos del sistema operativo, ¿evita el uso de intérpretes de shell y pasa los argumentos como un arreglo separado?

---

## 5. Lógica de Negocio y Concurrencia

*Piensa como un atacante intentando abusar de las reglas del negocio.*

* [ ] **Valores negativos o cero:** En operaciones financieras, carritos de compra o transferencias, ¿se bloquean explícitamente cantidades negativas, en cero o decimales infinitos?
* [ ] **Integridad de pasos:** Si un proceso tiene varios pasos (ej. Paso 1: Validar cupón -> Paso 2: Pagar -> Paso 3: Confirmar), ¿el código verifica que los pasos previos se hayan completado en el servidor?
* [ ] **Condiciones de carrera (Race Conditions):** En operaciones críticas con saldo o stock limitado (ej. canjear un cupón de un solo uso o retirar dinero), ¿se utilizan bloqueos transaccionales en la base de datos para evitar que dos peticiones simultáneas dupliquen el beneficio?

---

## 6. Manejo de Archivos

*Aplica siempre que el PR permita subir, leer o descargar archivos.*

* [ ] **Validación de contenido real:** ¿Se valida el tipo de archivo inspeccionando su contenido (magic bytes / MIME type real) y no solo confiando en la extensión del nombre (`.jpg`, `.pdf`)?
* [ ] **Prevención de Path Traversal:** Al guardar o leer archivos, ¿el código genera un nombre aleatorio interno (ej. UUID) en lugar de usar el nombre original enviado por el usuario (evitando rutas como `../../etc/passwd`)?
* [ ] **Almacenamiento aislado:** ¿Los archivos subidos se guardan fuera del directorio público de ejecución del servidor (idealmente en un bucket privado como S3)?

---

## 7. Criptografía, Secretos y Respuestas

*Revisa lo que el sistema guarda, oculta y devuelve al exterior.*

* [ ] **Aleatoriedad segura:** Para generar tokens, códigos de recuperación o identificadores impredecibles, ¿se usa un generador criptográficamente seguro (CSPRNG) en lugar de funciones matemáticas estándar (como `Math.random()` o `random()`)?
* [ ] **Exposición excesiva de datos:** ¿La API devuelve únicamente los campos que el cliente necesita, o está devolviendo el objeto completo de la base de datos (incluyendo hashes de contraseñas, tokens internos o datos de auditoría)?
* [ ] **Manejo limpio de errores:** En los bloques `try / catch`, ¿se evita devolver el mensaje técnico de la excepción o el *stack trace* en la respuesta HTTP hacia el cliente?

<!-- 

Cómo usarlo en 5 minutos por revisión

1. No leas el archivo de arriba a abajo: Ve directo a los archivos modificados que tocan rutas (controllers), servicios de negocio o consultas a base de datos.
2. Busca las "entradas": Ubica dónde llegan los datos del usuario en ese archivo y síguelos con la vista hasta que se guardan o se devuelven.
3. Aplica solo las secciones relevantes: Si el PR no sube archivos, salta la sección 6. Si no toca base de datos, salta la 4. Así mantienes tu revisión ágil y precisa.
 -->