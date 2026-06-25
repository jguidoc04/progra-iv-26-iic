# Guía de JSON en Java con Gson (`com.google.code.gson`)

---

## 1. ¿Qué es Gson?

**Gson** es una librería de Google que permite:
- Convertir objetos Java → JSON (**serialización**)
- Convertir JSON → objetos Java (**deserialización**)

---

## 2. Dependencia Maven

Agrega esto en tu `pom.xml`:

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.10.1</version>
</dependency>
```

O descarga el `.jar` desde [mvnrepository.com](https://mvnrepository.com/artifact/com.google.code.gson/gson)

---

## 3. Clase `Estudiante`

```java
public class Estudiante {
    private int id;
    private String nombre;
    private String carrera;
    private double promedio;

    // Constructor
    public Estudiante(int id, String nombre, String carrera, double promedio) {
        this.id = id;
        this.nombre = nombre;
        this.carrera = carrera;
        this.promedio = promedio;
    }

    // Getters
    public int getId() { return id; }
    public String getNombre() { return nombre; }
    public String getCarrera() { return carrera; }
    public double getPromedio() { return promedio; }

    @Override
    public String toString() {
        return "Estudiante{id=" + id + ", nombre='" + nombre + "', carrera='" + carrera + "', promedio=" + promedio + "}";
    }
}
```

---

## 4. Serialización: Objeto → JSON

### 4.1 Un solo objeto

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class EjemploSerial {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        Estudiante e = new Estudiante(1, "Ana López", "Informática", 9.2);
        String json = gson.toJson(e);

        System.out.println(json);
    }
}
```

**Salida:**
```json
{
  "id": 1,
  "nombre": "Ana López",
  "carrera": "Informática",
  "promedio": 9.2
}
```

---

### 4.2 Array/Lista de objetos → JSON

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import java.util.ArrayList;
import java.util.List;

public class EjemploLista {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        List<Estudiante> lista = new ArrayList<>();
        lista.add(new Estudiante(1, "Ana López",    "Informática",  9.2));
        lista.add(new Estudiante(2, "Carlos Pérez", "Electrónica",  8.7));
        lista.add(new Estudiante(3, "María Ruiz",   "Civil",        7.5));

        String json = gson.toJson(lista);
        System.out.println(json);
    }
}
```

**Salida:**
```json
[
  { "id": 1, "nombre": "Ana López",    "carrera": "Informática", "promedio": 9.2 },
  { "id": 2, "nombre": "Carlos Pérez", "carrera": "Electrónica", "promedio": 8.7 },
  { "id": 3, "nombre": "María Ruiz",   "carrera": "Civil",       "promedio": 7.5 }
]
```

---

## 5. Guardar JSON en un archivo `.json`

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class GuardarJSON {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        List<Estudiante> lista = new ArrayList<>();
        lista.add(new Estudiante(1, "Ana López",    "Informática",  9.2));
        lista.add(new Estudiante(2, "Carlos Pérez", "Electrónica",  8.7));
        lista.add(new Estudiante(3, "María Ruiz",   "Civil",        7.5));

        try (FileWriter writer = new FileWriter("estudiantes.json")) {
            gson.toJson(lista, writer);
            System.out.println("Archivo guardado correctamente.");
        } catch (IOException e) {
            System.out.println("Error al guardar: " + e.getMessage());
        }
    }
}
```

---

## 6. Deserialización: JSON → Lista de objetos

```java
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import java.io.FileReader;
import java.io.IOException;
import java.lang.reflect.Type;
import java.util.List;

public class LeerJSON {
    public static void main(String[] args) {
        Gson gson = new Gson();

        try (FileReader reader = new FileReader("estudiantes.json")) {
            // TypeToken indica a Gson el tipo exacto de la lista
            Type tipo = new TypeToken<List<Estudiante>>() {}.getType();
            List<Estudiante> lista = gson.fromJson(reader, tipo);

            for (Estudiante e : lista) {
                System.out.println(e);
            }
        } catch (IOException e) {
            System.out.println("Error al leer: " + e.getMessage());
        }
    }
}
```

---

## 7. CRUD completo en JSON

```java
import com.google.gson.*;
import com.google.gson.reflect.TypeToken;
import java.io.*;
import java.lang.reflect.Type;
import java.util.*;

public class GestorEstudiantes {
    private static final String ARCHIVO = "estudiantes.json";
    private static final Gson gson = new GsonBuilder().setPrettyPrinting().create();

    // Cargar lista desde archivo
    public static List<Estudiante> cargar() {
        try (FileReader reader = new FileReader(ARCHIVO)) {
            Type tipo = new TypeToken<List<Estudiante>>() {}.getType();
            List<Estudiante> lista = gson.fromJson(reader, tipo);
            return lista != null ? lista : new ArrayList<>();
        } catch (IOException e) {
            return new ArrayList<>(); // Si no existe el archivo, retorna lista vacía
        }
    }

    // Guardar lista en archivo
    public static void guardar(List<Estudiante> lista) {
        try (FileWriter writer = new FileWriter(ARCHIVO)) {
            gson.toJson(lista, writer);
        } catch (IOException e) {
            System.out.println("Error al guardar: " + e.getMessage());
        }
    }

    // Agregar estudiante
    public static void agregar(Estudiante e) {
        List<Estudiante> lista = cargar();
        lista.add(e);
        guardar(lista);
    }

    // Eliminar por ID
    public static void eliminar(int id) {
        List<Estudiante> lista = cargar();
        lista.removeIf(e -> e.getId() == id);
        guardar(lista);
    }

    // Buscar por ID
    public static Estudiante buscar(int id) {
        return cargar().stream()
                .filter(e -> e.getId() == id)
                .findFirst()
                .orElse(null);
    }

    public static void main(String[] args) {
        agregar(new Estudiante(1, "Ana López",    "Informática", 9.2));
        agregar(new Estudiante(2, "Carlos Pérez", "Electrónica", 8.7));
        agregar(new Estudiante(3, "María Ruiz",   "Civil",       7.5));

        System.out.println("Lista completa:");
        cargar().forEach(System.out::println);

        eliminar(2);
        System.out.println("\n Después de eliminar ID 2:");
        cargar().forEach(System.out::println);

        Estudiante encontrado = buscar(1);
        System.out.println("\n Buscando ID 1: " + encontrado);
    }
}
```

---

## 8. Tabla resumen de métodos Gson

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| `toJson(objeto)` | Objeto → String JSON | `gson.toJson(e)` |
| `toJson(objeto, writer)` | Objeto → Archivo | `gson.toJson(lista, writer)` |
| `fromJson(json, Clase.class)` | String JSON → Objeto | `gson.fromJson(json, Estudiante.class)` |
| `fromJson(reader, tipo)` | Archivo → Lista | `gson.fromJson(reader, tipo)` |
| `setPrettyPrinting()` | JSON con formato legible | `new GsonBuilder().setPrettyPrinting().create()` |

---

## 9.  Errores comunes

| Error | Causa | Solución |
|-------|-------|----------|
| `JsonSyntaxException` | JSON malformado | Verificar comillas y llaves |
| `FileNotFoundException` | Archivo no existe al leer | Verificar ruta y que el archivo fue creado |
| Lista devuelve `null` | JSON vacío o nulo | Validar con `lista != null ? lista : new ArrayList<>()` |
| Tipos incorrectos | Usar `Clase.class` en vez de `TypeToken` para listas | Usar `TypeToken<List<T>>` para colecciones |

---

## 10. Práctica: Sistema de Registro de Productos

### Enunciado

Desarrolla una aplicación Java que gestione el registro de productos mediante un archivo JSON usando la librería Gson.

### Requerimientos

1. **Clase `Producto`** con atributos: `id`, `nombre`, `precio`, `cantidad`.
2. **Menú de consola** con las opciones:
   - [1] Registrar nuevo producto
   - [2] Listar todos los productos
   - [3] Buscar producto por ID
   - [4] Eliminar producto por ID
   - [0] Salir
3. **Persistencia**: todos los cambios deben guardarse/cargarse desde `productos.json`

### Criterios de evaluación

| Criterio | Puntos |
|----------|--------|
| Clase `Producto` con atributos y constructor correctos | 10 pts |
| Guardar lista en JSON correctamente con Gson | 20 pts |
| Cargar lista desde JSON correctamente (TypeToken) | 20 pts |
| Menú funcional con todas las opciones | 25 pts |
| Manejo de errores (archivo no encontrado, ID inválido) | 15 pts |
| Cálculo correcto del promedio general | 10 pts |
| **Total** | **100 pts** |

### Entrega

- Proyecto Java con todos los archivos fuente (`.java`)
- Archivo `estudiantes.json` generado tras al menos 3 registros
- Captura de pantalla del menú en ejecución

---

*Guía elaborada para el curso de (ISB-21) PROGRAMACIÓN IV*
