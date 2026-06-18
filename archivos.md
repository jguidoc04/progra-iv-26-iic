# Guía de Manejo de Archivos JSON en Java

## Introducción

JSON (**JavaScript Object Notation**) es uno de los formatos más utilizados para almacenar e intercambiar información debido a que es ligero, legible y compatible con múltiples lenguajes.

En Java, una de las librerías más utilizadas para trabajar con JSON es **Jackson**.

---

# 1. Dependencias

Si utilizas Maven, agrega la siguiente dependencia:

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.18.0</version>
</dependency>
```

---

# 2. Crear una Clase Java

Supongamos una clase `Persona`.

```java
public class Persona {

    private String nombre;
    private int edad;

    public Persona() {
    }

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public int getEdad() {
        return edad;
    }

    public void setEdad(int edad) {
        this.edad = edad;
    }
}
```

---

# 3. Guardar un Objeto en JSON

## Código Java

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;

public class GuardarObjeto {

    public static void main(String[] args) throws Exception {

        Persona persona = new Persona("Jonathan", 30);

        ObjectMapper mapper = new ObjectMapper();

        mapper.writerWithDefaultPrettyPrinter()
              .writeValue(new File("persona.json"), persona);

        System.out.println("Archivo guardado");
    }
}
```

## Resultado

```json
{
  "nombre": "Jonathan",
  "edad": 30
}
```

---

# 4. Leer un Objeto desde JSON

## Archivo persona.json

```json
{
  "nombre": "Jonathan",
  "edad": 30
}
```

## Código Java

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;

public class LeerObjeto {

    public static void main(String[] args) throws Exception {

        ObjectMapper mapper = new ObjectMapper();

        Persona persona = mapper.readValue(
                new File("persona.json"),
                Persona.class);

        System.out.println(persona.getNombre());
        System.out.println(persona.getEdad());
    }
}
```

## Salida

```text
Jonathan
30
```

---

# 5. Guardar un Array de Objetos

## Crear Lista

```java
List<Persona> personas = new ArrayList<>();

personas.add(new Persona("Juan", 25));
personas.add(new Persona("María", 28));
personas.add(new Persona("Pedro", 35));
```

## Guardar JSON

```java
ObjectMapper mapper = new ObjectMapper();

mapper.writerWithDefaultPrettyPrinter()
      .writeValue(new File("personas.json"), personas);
```

## Resultado

```json
[
  {
    "nombre": "Juan",
    "edad": 25
  },
  {
    "nombre": "María",
    "edad": 28
  },
  {
    "nombre": "Pedro",
    "edad": 35
  }
]
```

---

# 6. Leer un Array de Objetos

```java
import com.fasterxml.jackson.core.type.TypeReference;

ObjectMapper mapper = new ObjectMapper();

List<Persona> personas = mapper.readValue(
        new File("personas.json"),
        new TypeReference<List<Persona>>() {}
);

for(Persona p : personas){
    System.out.println(p.getNombre());
}
```

## Salida

```text
Juan
María
Pedro
```

---

# 7. JSON con Objetos Anidados

## Clase Dirección

```java
public class Direccion {

    private String ciudad;
    private String pais;

    public Direccion() {}

    public Direccion(String ciudad, String pais) {
        this.ciudad = ciudad;
        this.pais = pais;
    }

    // getters y setters
}
```

## Clase Persona

```java
public class Persona {

    private String nombre;
    private int edad;
    private Direccion direccion;

    public Persona() {}

    public Persona(String nombre,
                   int edad,
                   Direccion direccion) {

        this.nombre = nombre;
        this.edad = edad;
        this.direccion = direccion;
    }

    // getters y setters
}
```

### Crear Objeto

```java
Direccion dir = new Direccion(
        "San José",
        "Costa Rica");

Persona persona = new Persona(
        "Jonathan",
        30,
        dir);
```

### Guardar

```java
mapper.writerWithDefaultPrettyPrinter()
      .writeValue(
          new File("personaCompleta.json"),
          persona);
```

### Resultado

```json
{
  "nombre": "Jonathan",
  "edad": 30,
  "direccion": {
    "ciudad": "San José",
    "pais": "Costa Rica"
  }
}
```

---

# 8. Array Dentro de un Objeto

## Clase Curso

```java
public class Curso {

    private String nombre;

    public Curso() {}

    public Curso(String nombre) {
        this.nombre = nombre;
    }

    // getters y setters
}
```

## Clase Estudiante

```java
import java.util.List;

public class Estudiante {

    private String nombre;
    private List<Curso> cursos;

    public Estudiante() {}

    public Estudiante(
            String nombre,
            List<Curso> cursos) {

        this.nombre = nombre;
        this.cursos = cursos;
    }

    // getters y setters
}
```

### Crear Objeto

```java
List<Curso> cursos = Arrays.asList(
        new Curso("Java"),
        new Curso("Bases de Datos"),
        new Curso("Redes")
);

Estudiante estudiante =
        new Estudiante(
                "Jonathan",
                cursos);
```

### Resultado JSON

```json
{
  "nombre": "Jonathan",
  "cursos": [
    {
      "nombre": "Java"
    },
    {
      "nombre": "Bases de Datos"
    },
    {
      "nombre": "Redes"
    }
  ]
}
```

---

# 9. Array de Objetos Complejos

## Clases

```java
public class Producto {

    private int id;
    private String nombre;
    private double precio;

    public Producto() {}

    public Producto(
            int id,
            String nombre,
            double precio) {

        this.id = id;
        this.nombre = nombre;
        this.precio = precio;
    }

    // getters y setters
}
```

### Crear Lista

```java
List<Producto> productos =
        new ArrayList<>();

productos.add(
        new Producto(
                1,
                "Laptop",
                850.00));

productos.add(
        new Producto(
                2,
                "Mouse",
                25.00));
```

### JSON

```json
[
  {
    "id": 1,
    "nombre": "Laptop",
    "precio": 850.0
  },
  {
    "id": 2,
    "nombre": "Mouse",
    "precio": 25.0
  }
]
```

---

# 10. Agregar un Registro a un JSON Existente

### Leer

```java
List<Persona> personas =
        mapper.readValue(
                new File("personas.json"),
                new TypeReference<List<Persona>>() {});
```

### Agregar

```java
personas.add(
        new Persona(
                "María",
                25));
```

### Guardar

```java
mapper.writerWithDefaultPrettyPrinter()
      .writeValue(
              new File("personas.json"),
              personas);
```

---

# 11. Convertir Objeto a JSON String

```java
Persona persona =
        new Persona(
                "Jonathan",
                30);

String json =
        mapper.writeValueAsString(
                persona);

System.out.println(json);
```

### Resultado

```json
{"nombre":"Jonathan","edad":30}
```

---

# 12. Convertir JSON String a Objeto

```java
String json =
        "{\"nombre\":\"Jonathan\",\"edad\":30}";

Persona persona =
        mapper.readValue(
                json,
                Persona.class);

System.out.println(
        persona.getNombre());
```

---

# 13. Uso de Map para JSON Dinámico

Cuando no conocemos la estructura exacta.

```java
Map<String, Object> datos =
        new HashMap<>();

datos.put("nombre", "Jonathan");
datos.put("edad", 30);
datos.put("activo", true);

mapper.writerWithDefaultPrettyPrinter()
      .writeValue(
              new File("datos.json"),
              datos);
```

### Resultado

```json
{
  "nombre": "Jonathan",
  "edad": 30,
  "activo": true
}
```

---

# 14. Leer JSON Dinámico

```java
Map<String, Object> datos =
        mapper.readValue(
                new File("datos.json"),
                Map.class);

System.out.println(
        datos.get("nombre"));
```

---

# 15. Ejemplo Real: Sistema de Estudiantes

## JSON

```json
{
  "estudiantes": [
    {
      "id": 1,
      "nombre": "Juan"
    },
    {
      "id": 2,
      "nombre": "María"
    }
  ]
}
```

## Clase Estudiante

```java
public class Estudiante {

    private int id;
    private String nombre;

    public Estudiante() {}

    // getters y setters
}
```

## Clase Universidad

```java
import java.util.List;

public class Universidad {

    private List<Estudiante> estudiantes;

    public Universidad() {}

    // getters y setters
}
```

## Leer

```java
Universidad universidad =
        mapper.readValue(
                new File("universidad.json"),
                Universidad.class);

for (Estudiante e :
        universidad.getEstudiantes()) {

    System.out.println(
            e.getNombre());
}
```

---

# Buenas Prácticas

## Constructor vacío

```java
public Persona() {
}
```

## Utilizar List

```java
private List<Persona> personas;
```

## Formatear JSON

```java
mapper.writerWithDefaultPrettyPrinter()
```

## Manejar excepciones

```java
try {

    // código

} catch (Exception e) {

    e.printStackTrace();
}
```

## Mantener una clase por entidad

```text
Persona.java
Direccion.java
Curso.java
Estudiante.java
Producto.java
```

---

# Resumen

| Operación | Método |
|------------|---------|
| Guardar objeto | writeValue() |
| Leer objeto | readValue() |
| Objeto → JSON String | writeValueAsString() |
| JSON String → Objeto | readValue() |
| Guardar lista | writeValue() |
| Leer lista | TypeReference<List<T>>() {} |
| JSON bonito | writerWithDefaultPrettyPrinter() |
| JSON dinámico | Map<String,Object> |
| Array de objetos | List<T> |
| Objeto anidado | Clase dentro de otra clase |

## Métodos Más Utilizados

```java
ObjectMapper mapper = new ObjectMapper();
```

Guardar:

```java
mapper.writeValue(
        new File("archivo.json"),
        objeto);
```

Leer:

```java
MiClase obj =
        mapper.readValue(
                new File("archivo.json"),
                MiClase.class);
```

Guardar lista:

```java
mapper.writeValue(
        new File("lista.json"),
        lista);
```

Leer lista:

```java
List<MiClase> lista =
        mapper.readValue(
                new File("lista.json"),
                new TypeReference<List<MiClase>>() {});
```