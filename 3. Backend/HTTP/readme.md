# Springboot

Vamos a iniciar con Springboot, para ello haremos las siguiente configuraciones

# Agregue el padre al proyecto Maven

Use el Springboot Initializer para crear un nuevo proyecto desde IntellJ Ultimate </br></br>

A la lista de dependencias, agregue
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
De esta forma nuestro repositorio tendrá lo necesario para comenzar un RestAPI HTTP

### Punto de inicio
Método main de nuestro repo
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class App extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

}
```

Ya con todas las condiciones iniciales puede agregar un controller
```
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @GetMapping("hello")
    public String hello() {
        return "Integrador 1";
    }
}

```



# Leer Datos desde el Request en Spring Boot

En Spring Boot, podemos extraer datos de las peticiones HTTP de varias maneras. A continuación, se presentan las formas más comunes:

## 1. Lectura de Headers
Podemos leer los headers de la petición utilizando la anotación `@RequestHeader`.

```java
@RestController
@RequestMapping("/api")
public class HeaderController {

    @GetMapping("/headers")
    public ResponseEntity<String> getHeaders(@RequestHeader("User-Agent") String userAgent) {
        return ResponseEntity.ok("User-Agent: " + userAgent);
    }
}
```

- `@RequestHeader("User-Agent")`: Extrae el valor del header `User-Agent` enviado en la petición.
- Retorna el valor del header en la respuesta.

---

## 2. Path Variables
Podemos extraer valores desde la URL usando `@PathVariable`.

```java
@RestController
@RequestMapping("/api")
public class PathVariableController {

    @GetMapping("/users/{id}")
    public ResponseEntity<String> getUserById(@PathVariable("id") Long userId) {
        return ResponseEntity.ok("Usuario con ID: " + userId);
    }
}
```

- `@PathVariable("id")`: Captura el valor `{id}` desde la URL.
- Retorna el ID en la respuesta.

---

## 3. Request Params
Podemos obtener parámetros de consulta (`query params`) con `@RequestParam`.

```java
@RestController
@RequestMapping("/api")
public class RequestParamController {

    @GetMapping("/search")
    public ResponseEntity<String> search(@RequestParam("query") String query) {
        return ResponseEntity.ok("Búsqueda: " + query);
    }
}
```

- `@RequestParam("query")`: Captura el parámetro de consulta `query`.
- Retorna el valor del parámetro en la respuesta.

---

## 4. Body en JSON
Para recibir datos en formato JSON, utilizamos `@RequestBody`.



```java
@RestController
@RequestMapping("/api")
public class RequestBodyController {

    @PostMapping("/users")
    public ResponseEntity<String> createUser(@RequestBody User user) {
        return ResponseEntity.ok("Usuario creado: " + user.getName() + ", Edad: " + user.getAge());
    }

}
```

```java
public class User {

    private String name;
    private int age;

    // Getters y setters
        
}
```

- `@RequestBody`: Convierte el JSON enviado en la petición a un objeto Java.
- `User`: Clase modelo para deserializar los datos JSON.
- Retorna la información del usuario recibido.



# Archivo de configuración
En el archivo de configuración se pueden cambiar parámetros estáticos del servidor. Como por ejemplo el puerto por el que escucha las solicitudes
```
server.port=8081
```


# HTTP Springboot Client
Springboot también puede ser cliente de otro API si se requiere

Para eso debe crear un archivo de configuración en un paquete config
```java
@Configuration
public class AppConfig {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```
Luego, puede usar algún método que haga el llamado. Por ejemplo un POST Request
```java
    public String httpRequest(String url, String body) throws IOException {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Content-Type", "application/json; UTF-8");

        HttpEntity<String> request = new HttpEntity<>(json, headers);
        ResponseEntity<String> response = restTemplate.postForEntity(url, request, String.class);

        if (response.getStatusCode() == HttpStatus.OK) {
            return response.getBody();
        } else {
            throw new IOException("HTTP Error: " + response.getStatusCode() + ", " + response.getBody());
        }
    }
```

# Comando útiles
Liste los programas corriendo sobre un puerto<br><br>
Unix
```
lsof -i:8080
```
Windows
```
netstat -ano | findstr :<PORT>
```

Mate el proceso en el puerto elegido<br><br>
Unix
```
kill $(lsof -t -i:8080)
```
Windows
```
taskkill /PID <PID> /F
```



