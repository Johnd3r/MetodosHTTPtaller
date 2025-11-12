## Servidor HTTP de Minecraft con Spring Boot

A continuación se muestra cómo replicar el ejemplo anterior usando Spring Boot para manejar los métodos HTTP (GET, POST, PUT, DELETE, OPTIONS, HEAD, CONNECT, TRACE).

## **CLASE PRINCIPAL**
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MinecraftHttpApp {
    public static void main(String[] args) {
        SpringApplication.run(MinecraftHttpApp.class, args);
        System.out.println("🌐 Servidor Minecraft corriendo en http://localhost:8080/minecraft");
    }
}

```
## **CONTROLADOR HTTP**
```java
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/minecraft")
public class MinecraftController {

    @GetMapping
    public String getInfo() {
        return "🔍 Consultando información del jugador Steve...";
    }

    @PostMapping
    public String postBlock() {
        return "🧱 Nuevo bloque colocado en el mundo.";
    }

    @PutMapping
    public String putBlock() {
        return "⚒️ Se actualizó la posición del bloque.";
    }

    @DeleteMapping
    public String deleteBlock() {
        return "💥 Bloque eliminado.";
    }

    @RequestMapping(method = RequestMethod.OPTIONS)
    public ResponseEntity<String> options() {
        HttpHeaders headers = new HttpHeaders();
        headers.add("Allow", "GET, POST, PUT, DELETE, OPTIONS, HEAD, CONNECT, TRACE");
        return ResponseEntity.ok().headers(headers).body("📜 Métodos permitidos listados.");
    }

    @RequestMapping(method = RequestMethod.HEAD)
    public ResponseEntity<Void> head() {
        return ResponseEntity.ok().build(); // Solo cabeceras, sin cuerpo
    }

    @RequestMapping(method = RequestMethod.CONNECT)
    public String connect() {
        return "🔗 Estableciendo túnel con el servidor del Nether...";
    }

    @RequestMapping(method = RequestMethod.TRACE)
    public String trace() {
        return "📡 Eco del cliente TRACE recibido correctamente.";
    }
}

```

## RESULTADO:

Al ejecutarse la aplicacion de ejemplo que tenemos ahi y acceder a:

- **GET** → `http://localhost:8080/minecraft`  
  *"Consultando información del jugador Steve..."*

- **POST** → `http://localhost:8080/minecraft`  
  *"Nuevo bloque colocado en el mundo."*

- **PUT** → *Actualiza un bloque*

- **DELETE** → *Elimina una estructura*

- **OPTIONS** → *Lista los métodos disponibles*

- **HEAD** → *Responde solo con encabezados*

- **CONNECT / TRACE** → *Simulan conexión y diagnóstico*
