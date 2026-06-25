# Práctica N°6 — Interfaces Gráficas con Java Swing

**Curso:** Programación Orientada a Objetos  
**Unidad:** 6 — Java Swing  
**Valor:** 10 puntos  
**Modalidad:** Individual  
**Entrega:** Subir proyecto `.zip` al aula virtual antes de la fecha indicada por el docente

---

## Objetivos

- Diseñar interfaces gráficas de escritorio usando componentes Swing.
- Aplicar eventos de usuario mediante `ActionListener` y otros listeners.
- Integrar la lógica de negocio con la capa de presentación usando el patrón MVC básico.
- Manejar tablas (`JTable`) y formularios dinámicos.

---

## Contexto

La empresa ficticia **TechStore CR** necesita un sistema de escritorio para gestionar su inventario de productos tecnológicos. Se te ha asignado construir la interfaz gráfica de dicho sistema usando **Java Swing**.

---

## Ejercicio 1 — Ventana Principal con Menú (2 pts)

Crea una clase `VentanaPrincipal` que extienda `JFrame` y cumpla con los siguientes requisitos:

### Requerimientos

- Título de la ventana: `"TechStore CR — Sistema de Inventario"`
- Tamaño: `900 x 600` píxeles, centrada en pantalla.
- Operación de cierre: debe cerrar la aplicación completamente (`EXIT_ON_CLOSE`).
- Barra de menú (`JMenuBar`) con los siguientes menús:

| Menú | Ítems |
|---|---|
| **Archivo** | Nuevo, Abrir, Guardar, `---`, Salir |
| **Productos** | Agregar Producto, Ver Inventario |
| **Ayuda** | Acerca de... |

- El ítem **Salir** debe cerrar la aplicación con confirmación (`JOptionPane`).
- El ítem **Acerca de...** debe mostrar un `JDialog` con el nombre del sistema y el autor.

### Código base sugerido

```java
public class VentanaPrincipal extends JFrame {
    public VentanaPrincipal() {
        setTitle("TechStore CR — Sistema de Inventario");
        setSize(900, 600);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        initMenu();
        setVisible(true);
    }

    private void initMenu() {
        // TODO: construir y agregar JMenuBar
    }
}
```

---

## Ejercicio 2 — Formulario de Registro de Producto (3 pts)

Crea un panel (`JPanel`) o un `JDialog` llamado `FormularioProducto` que permita registrar un nuevo producto con los siguientes campos:

### Campos del formulario

| Campo | Componente Swing |
|---|---|
| Código | `JTextField` (solo números, máx. 6 dígitos) |
| Nombre del producto | `JTextField` |
| Categoría | `JComboBox` (Laptop, Tablet, Celular, Accesorio, Otro) |
| Precio (₡) | `JTextField` (validar que sea número decimal positivo) |
| Cantidad en stock | `JSpinner` (mínimo 0, máximo 9999) |
| Disponible | `JCheckBox` |

### Botones

- **Guardar:** valida los campos y agrega el producto a la lista.
- **Limpiar:** restablece todos los campos a sus valores por defecto.
- **Cancelar:** cierra el formulario sin guardar.

### Validaciones requeridas

- Ningún campo de texto puede estar vacío.
- El código debe contener solo dígitos.
- El precio debe ser un número decimal mayor que 0.
- Mostrar mensajes de error con `JOptionPane.showMessageDialog(...)`.

---

## Ejercicio 3 — Tabla de Inventario con JTable (3 pts)

Crea un panel `PanelInventario` que muestre el inventario de productos en una `JTable`.

### Columnas de la tabla

| # | Columna | Tipo de dato |
|---|---|---|
| 1 | Código | String |
| 2 | Nombre | String |
| 3 | Categoría | String |
| 4 | Precio (₡) | Double |
| 5 | Stock | Integer |
| 6 | Disponible | Boolean (renderizado como checkbox) |

### Requerimientos funcionales

- Usa un `DefaultTableModel` para manejar los datos.
- La tabla **no debe ser editable** directamente (sobrescribe `isCellEditable`).
- Agrega un campo de búsqueda (`JTextField`) encima de la tabla que filtre los productos por nombre en tiempo real.
- Incluye un botón **Eliminar** que elimine la fila seleccionada con confirmación.
- Incluye un botón **Editar** que cargue los datos del producto seleccionado en el `FormularioProducto`.

### Código base sugerido

```java
public class PanelInventario extends JPanel {
    private JTable tablaProductos;
    private DefaultTableModel modelo;

    public PanelInventario() {
        String[] columnas = {"Código", "Nombre", "Categoría", "Precio (₡)", "Stock", "Disponible"};
        modelo = new DefaultTableModel(columnas, 0) {
            @Override
            public boolean isCellEditable(int row, int column) {
                return false;
            }
        };
        tablaProductos = new JTable(modelo);
        // TODO: configurar JScrollPane, botones y campo de búsqueda
    }

    public void agregarProducto(Producto p) {
        // TODO: agregar fila al modelo
    }
}
```

---

## Ejercicio 4 — Integración y Funcionalidad Final (2 pts)

Integra todos los componentes anteriores en la `VentanaPrincipal` y asegúrate de que el flujo completo funcione.

### Requerimientos de integración

- El menú **Agregar Producto** abre el `FormularioProducto`.
- Al guardar un producto desde el formulario, se refleja inmediatamente en la tabla del `PanelInventario`.
- El menú **Ver Inventario** cambia el panel visible al `PanelInventario` (usa `CardLayout` o `JTabbedPane`).
- Al iniciar la aplicación, carga **al menos 3 productos de ejemplo** en la tabla.
- La barra de estado (parte inferior de la ventana, `JLabel`) muestra el **total de productos** en inventario y se actualiza con cada cambio.

### Estructura de clase `Producto` (modelo)

```java
public class Producto {
    private String codigo;
    private String nombre;
    private String categoria;
    private double precio;
    private int stock;
    private boolean disponible;

    // Constructor, getters y setters
}
```

---

## Criterios de Evaluación

| Criterio | Descripción | Puntaje |
|---|---|---|
| Ejercicio 1 | Ventana principal con menú funcional y diálogo "Acerca de" | 2 pts |
| Ejercicio 2 | Formulario con validaciones completas | 3 pts |
| Ejercicio 3 | Tabla con búsqueda, eliminar y editar | 3 pts |
| Ejercicio 4 | Integración fluida y datos de ejemplo | 2 pts |
| **Total** | | **10 pts** |

> **Nota:** Se restará 1 punto si el proyecto no compila, o si la interfaz genera errores en tiempo de ejecución.

---

## Estructura del Proyecto

El proyecto debe seguir la siguiente estructura de paquetes:

```
techstore/
├── modelo/
│   └── Producto.java
├── vista/
│   ├── VentanaPrincipal.java
│   ├── FormularioProducto.java
│   └── PanelInventario.java
└── Main.java
```

---

## Recomendaciones

> 💡 **Tip:** Usa `SwingUtilities.invokeLater(...)` en el `main` para iniciar la interfaz de forma segura en el hilo de eventos de Swing.

> ⚠️ **Advertencia:** No uses `null` como `LayoutManager`. Utiliza `BorderLayout`, `GridBagLayout`, `FlowLayout` u otros gestores apropiados para mantener una interfaz responsiva.

> 🔍 **Para el filtro de búsqueda:** Investiga el uso de `DocumentListener` aplicado a un `JTextField` para reaccionar al cambio de texto en tiempo real.

---

## Preguntas de Reflexión

Responde brevemente en un archivo `reflexion.txt` dentro del proyecto:

1. ¿Qué ventaja ofrece separar la lógica del modelo (`Producto`) de la vista (`PanelInventario`)?
2. ¿Por qué se recomienda iniciar Swing dentro de `SwingUtilities.invokeLater`?
3. ¿Cómo podrías persistir los productos registrados para que no se pierdan al cerrar la aplicación?

---

*Guía elaborada para el curso de (ISB-21) PROGRAMACIÓN IV*
