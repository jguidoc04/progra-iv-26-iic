# Práctica: Diseño de Interfaces Gráficas con Java Swing usando el Editor GUI de NetBeans

**Curso:** Programación Orientada a Objetos
**Unidad:** Interfaces Gráficas de Usuario (GUI) con Java Swing
**Modalidad:** Práctica guiada paso a paso

---

## 1. Objetivos de aprendizaje

Al finalizar esta práctica, el estudiante será capaz de:

- Crear un proyecto Java con soporte para interfaces gráficas en NetBeans.
- Utilizar el editor visual (GUI Builder / Matisse) para diseñar formularios con componentes Swing.
- Configurar propiedades de los componentes desde el panel de propiedades.
- Generar y comprender el código Java que NetBeans crea automáticamente.
- Programar eventos (ActionListener) asociados a botones y otros componentes.
- Ejecutar y probar una aplicación de escritorio funcional.

---

## 2. Requisitos previos

- Tener instalado **Apache NetBeans IDE** (versión 12 o superior) con el módulo de soporte para Java SE.
- Tener instalado el **JDK** (Java Development Kit), versión 8 o superior.
- Conocimientos básicos de sintaxis de Java (clases, métodos, variables).
- No se requiere experiencia previa con Swing.

> **Nota:** Si NetBeans no muestra la opción "JFrame Form" al crear un archivo nuevo, verifique que el proyecto sea de tipo "Java Application" (no "Java with Ant" limitado, ni un proyecto Maven sin soporte de formularios configurado).

---

## 3. Introducción teórica breve

**Java Swing** es una biblioteca gráfica que forma parte de Java Foundation Classes (JFC), utilizada para construir interfaces de usuario de escritorio (ventanas, botones, campos de texto, tablas, etc.).

El **GUI Builder** de NetBeans (también conocido históricamente como **Matisse**) es un editor visual de arrastrar y soltar (*drag and drop*) que permite diseñar formularios Swing sin escribir manualmente el código de posicionamiento de los componentes. NetBeans genera automáticamente el código Java correspondiente dentro de un bloque especial marcado como:

```java
// <editor-fold defaultstate="collapsed" desc="Generated Code">
...
// </editor-fold>
```

Este bloque **no debe editarse manualmente**, ya que el editor visual lo sobrescribe cada vez que se guardan cambios en el diseño.

### Componentes Swing más comunes

| Componente | Descripción |
|---|---|
| `JFrame` | Ventana principal de la aplicación |
| `JPanel` | Contenedor para agrupar otros componentes |
| `JLabel` | Etiqueta de texto |
| `JTextField` | Campo de texto de una línea |
| `JButton` | Botón presionable |
| `JCheckBox` | Casilla de verificación |
| `JComboBox` | Lista desplegable |
| `JTable` | Tabla de datos |
| `JOptionPane` | Ventanas emergentes (mensajes, confirmaciones) |

---

## 4. Desarrollo paso a paso

### Paso 1: Crear el proyecto

1. Abra NetBeans.
2. Vaya a **File → New Project**.
3. Seleccione la categoría **Java with Ant** (o **Java Application** según la versión) → **Java Application**.
4. Asigne el nombre del proyecto: `PracticaSwingNetBeans`.
5. Desmarque la opción "Create Main Class" si desea agregar el formulario manualmente, o déjela marcada; no afecta el resultado final.
6. Haga clic en **Finish**.

### Paso 2: Crear el formulario (JFrame Form)

1. En el panel **Projects**, haga clic derecho sobre el paquete del proyecto (por ejemplo `practicaswingnetbeans`).
2. Seleccione **New → JFrame Form...**
3. Asigne el nombre de la clase: `VentanaPrincipal`.
4. Haga clic en **Finish**.

Al finalizar, NetBeans abrirá automáticamente el **editor visual**, mostrando una ventana en blanco lista para diseñar.

### Paso 3: Explorar el entorno del editor visual

El editor visual se compone de tres áreas principales:

- **Paleta de componentes** (generalmente a la derecha): contiene los controles Swing organizados por categorías (Swing Controls, Swing Containers, etc.).
- **Área de diseño** (centro): representa visualmente la ventana que se está construyendo.
- **Panel de propiedades** (inferior derecha): permite modificar las propiedades del componente seleccionado (texto, tamaño, color, nombre de variable, etc.).
- **Navegador de componentes (Inspector)**: muestra la jerarquía de componentes agregados al formulario.

### Paso 4: Agregar componentes al formulario

1. Desde la paleta, arrastre un componente **Label** hacia el área de diseño y colóquelo en la parte superior.
2. En el panel de propiedades, busque la propiedad `text` y cámbiela a: `Nombre:`
3. Arrastre un componente **Text Field** junto a la etiqueta anterior.
4. Haga clic derecho sobre el campo de texto → **Change Variable Name...** → asigne el nombre `txtNombre`.
5. Repita el proceso agregando:
   - Una segunda etiqueta con texto `Edad:`
   - Un segundo campo de texto con nombre de variable `txtEdad`
6. Arrastre un componente **Button** debajo de los campos anteriores.
7. Cambie su propiedad `text` a: `Saludar`
8. Cambie el nombre de variable del botón a `btnSaludar`.
9. Arrastre una tercera etiqueta debajo del botón, cambie su texto a un espacio vacío y su nombre de variable a `lblResultado`. Esta etiqueta se usará para mostrar el mensaje de salida.

> **Consejo:** Use las líneas guía color celeste que aparecen al mover los componentes; estas indican alineación automática respecto a otros elementos del formulario.

### Paso 5: Ajustar propiedades visuales (opcional)

1. Seleccione el `JFrame` (fondo del formulario) y modifique su propiedad `title` a: `Práctica Swing - Saludo`.
2. Seleccione el botón `btnSaludar` y, en la pestaña de propiedades, cambie la fuente (`font`) a negrita, tamaño 14.
3. Guarde los cambios con **Ctrl + S**.

### Paso 6: Programar el evento del botón

1. Haga doble clic sobre el botón `btnSaludar` en el área de diseño.
2. NetBeans cambiará automáticamente a la vista de **código fuente** y generará el método:

```java
private void btnSaludarActionPerformed(java.awt.event.ActionEvent evt) {
    // TODO agregar código aquí
}
```

3. Complete el método con la siguiente lógica:

```java
private void btnSaludarActionPerformed(java.awt.event.ActionEvent evt) {
    String nombre = txtNombre.getText();
    String edadTexto = txtEdad.getText();

    if (nombre.isEmpty() || edadTexto.isEmpty()) {
        JOptionPane.showMessageDialog(this,
            "Por favor complete todos los campos.",
            "Datos incompletos",
            JOptionPane.WARNING_MESSAGE);
        return;
    }

    try {
        int edad = Integer.parseInt(edadTexto);
        lblResultado.setText("Hola " + nombre + ", tienes " + edad + " años.");
    } catch (NumberFormatException e) {
        JOptionPane.showMessageDialog(this,
            "La edad debe ser un número entero.",
            "Error de formato",
            JOptionPane.ERROR_MESSAGE);
    }
}
```

4. Verifique que exista el import correspondiente al inicio del archivo:

```java
import javax.swing.JOptionPane;
```

### Paso 7: Ejecutar la aplicación

1. Haga clic derecho sobre la clase `VentanaPrincipal` en el panel Projects.
2. Seleccione **Run File** (o presione **Shift + F6**).
3. Pruebe la aplicación:
   - Ingrese un nombre y una edad válida → debe mostrarse el mensaje de saludo.
   - Deje un campo vacío → debe mostrarse la advertencia.
   - Ingrese texto no numérico en el campo edad → debe mostrarse el error de formato.

### Paso 8: Establecer la clase principal del proyecto (opcional)

Si desea que `VentanaPrincipal` se ejecute al correr todo el proyecto (**F6**), en lugar de solo el archivo:

1. Clic derecho sobre el proyecto → **Properties**.
2. En la categoría **Run**, cambie el campo **Main Class** a `practicaswingnetbeans.VentanaPrincipal`.
3. Haga clic en **OK**.

---

## 5. Ejercicio guiado adicional: Validación con JCheckBox y JComboBox

Amplíe el formulario anterior agregando:

1. Un `JComboBox` con nombre `cmbPais`, cargado con al menos 3 países (puede definir los valores en la propiedad `model` desde el editor de propiedades, opción "..." junto a `model`).
2. Un `JCheckBox` con nombre `chkAceptaTerminos` y texto `Acepto los términos y condiciones`.
3. Modifique el evento del botón `btnSaludar` para que:
   - Si el `JCheckBox` no está marcado, muestre un mensaje de advertencia y no continúe.
   - Incluya el país seleccionado en el mensaje final, usando `cmbPais.getSelectedItem()`.

---

## 6. Ejercicios para entregar

1. **Calculadora básica:** cree un formulario con dos campos numéricos, un `JComboBox` para elegir la operación (suma, resta, multiplicación, división) y un botón que calcule y muestre el resultado. Valide división entre cero.

2. **Formulario de registro:** diseñe un formulario con campos de nombre, correo electrónico y contraseña (use `JPasswordField`), un botón "Registrar" y validaciones básicas (campos no vacíos, formato de correo simple con `contains("@")`).

3. **Lista de tareas simple:** utilice un `JTextField`, un botón "Agregar" y un `JList` (o `JTextArea`) donde cada clic en el botón añada el texto ingresado a la lista.

Cada ejercicio debe entregarse como proyecto NetBeans completo (carpeta comprimida en `.zip`), incluyendo capturas de pantalla del diseño en el editor visual y de la ejecución.

---

## 7. Preguntas de repaso

1. ¿Qué diferencia existe entre `JFrame` y `JPanel`?
2. ¿Por qué no se debe editar manualmente el bloque de código generado por NetBeans?
3. ¿Qué método se ejecuta automáticamente al hacer doble clic sobre un botón en el editor visual?
4. ¿Qué clase se utiliza para mostrar ventanas emergentes de mensajes en Swing?
5. ¿Qué ocurre si se intenta convertir un texto no numérico con `Integer.parseInt()`?

---

## 8. Glosario

- **Swing:** biblioteca gráfica de Java para construir interfaces de escritorio.
- **GUI Builder / Matisse:** editor visual de NetBeans para diseñar formularios Swing mediante arrastrar y soltar.
- **ActionListener:** interfaz que permite capturar eventos de acción, como el clic de un botón.
- **JOptionPane:** clase de Swing utilizada para mostrar cuadros de diálogo emergentes.
- **NumberFormatException:** excepción lanzada cuando se intenta convertir un texto no numérico a un tipo numérico.

---

## 9. Rúbrica sugerida de evaluación

| Criterio | Puntos |
|---|---|
| Diseño correcto del formulario en el editor visual | 20 |
| Nombres de variables adecuados para los componentes | 15 |
| Implementación correcta del evento del botón | 25 |
| Validaciones de datos (campos vacíos, formato numérico) | 20 |
| Ejecución sin errores | 10 |
| Capturas de pantalla y organización de la entrega | 10 |
| **Total** | **100** |
