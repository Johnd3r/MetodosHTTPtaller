# MetodosHHTPtaller

Introducción
Esta guía resume cómo funcionan los métodos HTTP en el contexto de APIs de juegos como Minecraft y muestra ejemplos prácticos usando Java. Así puedes integrarlo fácilmente en tu README de GitHub para un proyecto de plugins, bots o integraciones con servidores de Minecraft.

# Taller: Métodos HTTP — Aplicación en Java con temática de Minecraft

---

## ¿Qué son los métodos HTTP?

Los **métodos HTTP** son comandos que indican **qué acción** se desea realizar sobre un recurso dentro de un servidor web.  
Son parte esencial del **protocolo HTTP (HyperText Transfer Protocol)**, que permite la comunicación entre clientes (navegadores, aplicaciones, juegos) y servidores en la web.

En el contexto de **Minecraft**, podrían servir para conectar el servidor del juego con una API externa, por ejemplo, para guardar información de jugadores, estadísticas o inventarios.

---

## Listado de métodos HTTP y su aplicabilidad

| **Método** | **Descripción** | **Cuándo usarlo** | **Ejemplo típico (Minecraft)** |
|:-----------:|:----------------|:------------------|:--------------------------------|
| **GET** | Solicita información de un recurso. | Para obtener datos sin modificarlos. | Obtener el inventario de un jugador. |
| **POST** | Envía datos al servidor para crear un recurso nuevo. | Cuando se registra un nuevo jugador o se envían estadísticas. | Crear una nueva puntuación en el servidor. |
| **PUT** | Reemplaza completamente un recurso existente. | Cuando se actualiza toda la información de un objeto. | Actualizar todos los datos de una aldea. |
| **PATCH** | Modifica parcialmente un recurso. | Cuando solo se cambia una parte de un objeto. | Subir el nivel de experiencia de un jugador. |
| **DELETE** | Elimina un recurso del servidor. | Cuando se elimina una entidad o registro. | Borrar un bloque o estructura guardada. |
| **OPTIONS** | Muestra los métodos disponibles para un recurso. | Para descubrir qué acciones están permitidas sobre una URL. | Verificar si un endpoint acepta `POST` o `DELETE`. |
| **HEAD** | Igual que GET, pero solo devuelve las cabeceras. | Para comprobar si un recurso existe o si un servidor está activo. | Revisar si el servidor de Minecraft responde. |
| **CONNECT** | Crea un túnel hacia el servidor (usado con HTTPS). | Para conexiones seguras o a través de proxies. | Conectar con un servidor seguro de Minecraft. |
| **TRACE** | Devuelve la solicitud recibida (depuración). | Para verificar el camino que sigue una petición. | Rastrear una solicitud desde el cliente al servidor. |

---

## Relación con arquitecturas web

Los métodos HTTP son la base de muchas **arquitecturas web**.  
En particular, se integran de forma natural con **REST (Representational State Transfer)**, que es el estándar moderno para APIs.

### **En una arquitectura REST**
Cada método HTTP representa una operación sobre un recurso:
- GET /jugadores → Obtener lista de jugadores
- POST /jugadores → Crear un nuevo jugador
- PUT /jugadores/1 → Reemplazar jugador con ID 1
- PATCH /jugadores/1 → Actualizar parcialmente el jugador con ID 1
- DELETE /jugadores/1 → Eliminar jugador con ID 1


### **En una arquitectura SOAP**
Aunque también usa HTTP, SOAP se basa en **mensajes XML** y casi siempre utiliza el método **POST** para enviar solicitudes.  
REST es más simple, liviano y usado hoy en día por APIs de videojuegos, apps y servidores.

---

##  Forma de uso — Ejemplos prácticos en Java (basados en Minecraft)

A continuación, algunos ejemplos de cómo se usan los métodos HTTP en Java, simulando un entorno donde el servidor de Minecraft se comunica con una API web.

---

### **Ejemplo con GET**

```java
// Obtener información de un jugador en Minecraft
HttpURLConnection connection = (HttpURLConnection)
    new URL("https://api.minecraftserver.com/jugadores/Steve").openConnection();

connection.setRequestMethod("GET");

if (connection.getResponseCode() == 200) {
    BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
    String response = reader.lines().collect(Collectors.joining());
    System.out.println("Datos del jugador: " + response);
}
```
### Ejemplo con POST
```java
// Registrar un nuevo jugador en el servidor
URL url = new URL("https://api.minecraftserver.com/jugadores");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();

connection.setRequestMethod("POST");
connection.setRequestProperty("Content-Type", "application/json");
connection.setDoOutput(true);

String jsonInput = "{\"nombre\":\"Alex\",\"nivel\":5,\"mundo\":\"Overworld\"}";

try (OutputStream os = connection.getOutputStream()) {
    os.write(jsonInput.getBytes());
}

if (connection.getResponseCode() == 201) {
    System.out.println("Jugador creado exitosamente.");
}
```

### Ejemplo con PATCH
```java
// Subir el nivel de experiencia de un jugador
URL url = new URL("https://api.minecraftserver.com/jugadores/Steve");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();

connection.setRequestMethod("PATCH");
connection.setRequestProperty("Content-Type", "application/json");
connection.setDoOutput(true);

String patchData = "{\"nivel\": 42}";

try (OutputStream os = connection.getOutputStream()) {
    os.write(patchData.getBytes());
}

if (connection.getResponseCode() == 200) {
    System.out.println("Nivel de Steve actualizado correctamente.");
}
```
### Ejemplo con DELETE
```java
// Eliminar una estructura del servidor
URL url = new URL("https://api.minecraftserver.com/estructuras/castillo-oscuro");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();

connection.setRequestMethod("DELETE");

if (connection.getResponseCode() == 204) {
    System.out.println("Estructura eliminada del servidor.");
}
```
# HEAD — Verificar si un recurso existe
```java
URL url = new URL("https://api.minecraftserver.com/mundos/Nether");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

conn.setRequestMethod("HEAD");

if (conn.getResponseCode() == 200) {
    System.out.println("El mundo Nether está disponible en el servidor.");
} else {
    System.out.println("El mundo Nether no existe o está offline.");
}
```

# CONNECT — Crear una conexión segura (HTTPS)
```java
URL url = new URL("https://secure.minecraftserver.com/login");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// CONNECT se usa internamente en HTTPS para establecer un túnel seguro.
// En Java, al usar https:// automáticamente se realiza un CONNECT.

System.out.println("Conexión segura establecida con el servidor de Minecraft (HTTPS).");
```
💡 Nota:
El método CONNECT normalmente no se invoca directamente en Java; se utiliza internamente por HTTPS para crear un túnel cifrado.
Sin embargo, es clave para entender cómo funcionan las conexiones seguras entre cliente y servidor.

# TRACE — Depurar la ruta de una solicitud
```java
URL url = new URL("https://api.minecraftserver.com/debug");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

conn.setRequestMethod("TRACE");

BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
String traceResponse = reader.lines().collect(Collectors.joining());

System.out.println("Respuesta TRACE del servidor:");
System.out.println(traceResponse);
```

### Resultado esperado:
```java
TRACE /debug HTTP/1.1
Host: api.minecraftserver.com
User-Agent: Java/17
```

### El método TRACE devuelve exactamente lo que el servidor recibió, útil para depurar errores de red o encabezados incorrectos.


