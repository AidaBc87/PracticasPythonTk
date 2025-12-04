Controles de **selección de opciones** (`Radiobutton`, `Checkbutton`) y el de **selección múltiple** (`Listbox`).
Crearemos una práctica de **"Pedido de Café Personalizado"** que combina estos nuevos controles con los que ya conocen (`Button` y `Label`).

-----

## 1\. 🎯 Objetivo de la Práctica: Pedido de Café ☕

El objetivo es crear una aplicación donde el usuario pueda configurar un pedido de café y ver un resumen en pantalla.

| Control | Función en el Programa |
| :--- | :--- |
| **Radiobutton** | Seleccionar **una única** opción (Tamaño del café: Pequeño, Mediano, Grande). |
| **Checkbutton** | Seleccionar **cero o más** opciones (Extras: Leche, Azúcar, Crema). |
| **Listbox** | Seleccionar **uno o más** tipos de grano (Espresso, Latte, Capuchino, etc.). |
| **Button** | Procesar el pedido. |
| **Label** | Mostrar el resumen del pedido. |

-----

## 2\. ⚙️ Preparación en VS Code

1.  Abre la carpeta de tu proyecto.
2.  Crea un nuevo archivo llamado `pedido_cafe.py`.

-----

## 3\. 🐍 El Código Paso a Paso

Copia el siguiente código en `pedido_cafe.py`.

### Sección 1: Importar y Definir Variables de Control

En Tkinter, los valores de los `Radiobutton` y `Checkbutton` no se obtienen directamente, sino que se enlazan a **variables especiales** (`IntVar` y `StringVar`).

```python
import tkinter as tk
from tkinter import END # Importamos END para limpiar el Listbox

# --- 1. CONFIGURACIÓN DE VARIABLES ESPECIALES TKINTER ---
# Estas variables almacenan el estado de los controles
# La variable de tamaño debe ser global para poder acceder a ella desde cualquier lugar
def configurar_variables(ventana_principal):
    # Para el Radiobutton (solo una opción se puede elegir, valor numérico)
    global tamaño_seleccionado
    tamaño_seleccionado = tk.IntVar(ventana_principal) 
    tamaño_seleccionado.set(1) # Valor inicial: 1 (Pequeño)

    # Para los Checkbuttons (0 o 1 para cada extra)
    global extra_leche, extra_azucar, extra_crema
    extra_leche = tk.IntVar(ventana_principal)
    extra_azucar = tk.IntVar(ventana_principal)
    extra_crema = tk.IntVar(ventana_principal)

# La definimos aquí para que esté disponible después de la ventana
tamaño_seleccionado = None
extra_leche = None
extra_azucar = None
extra_crema = None
```

-----

### Sección 2: La Lógica del Pedido

Esta función recopila la información de todos los controles (`Radiobutton`, `Checkbutton`, `Listbox`) y genera el mensaje final.

```python
def procesar_pedido():
    # --- A. OBTENER EL TAMAÑO (RADIOBUTTON) ---
    valor_tamaño = tamaño_seleccionado.get()
    if valor_tamaño == 1:
        tamaño = "Pequeño"
    elif valor_tamaño == 2:
        tamaño = "Mediano"
    else: # valor_tamaño == 3
        tamaño = "Grande"

    # --- B. OBTENER EXTRAS (CHECKBUTTONS) ---
    extras = []
    if extra_leche.get() == 1:
        extras.append("con Leche")
    if extra_azucar.get() == 1:
        extras.append("con Azúcar")
    if extra_crema.get() == 1:
        extras.append("con Crema")
    
    # Formatear la lista de extras
    extras_str = ", ".join(extras) if extras else "sin Extras"
    
    # --- C. OBTENER GRANO (LISTBOX) ---
    # .curselection() devuelve los índices seleccionados (ej: (0, 2))
    indices_grano = listbox_grano.curselection()
    
    if not indices_grano:
        # Si no se selecciona nada, asumimos Espresso
        tipo_grano = "Espresso estándar"
    else:
        # Recorremos los índices y obtenemos el texto de cada uno
        granos_seleccionados = [listbox_grano.get(i) for i in indices_grano]
        tipo_grano = " y ".join(granos_seleccionados)

    # --- D. GENERAR EL RESUMEN FINAL ---
    resumen = f"Tu Pedido:\nCafé {tamaño} de {tipo_grano}\n{extras_str}"
    
    # Actualizar el Label de resultado
    label_resumen.config(text=resumen, fg="brown")
```

-----

### Sección 3: Configurar la Ventana y Widgets

```python
# --- 2. CREACIÓN DE LA VENTANA Y DISPOSICIÓN ---
ventana = tk.Tk()
ventana.title("Personalizador de Café Tkinter")
ventana.geometry("500x550")
configurar_variables(ventana) # Llamamos a la función para inicializar variables

# Título General (Label)
tk.Label(ventana, text="CONFIGURACIÓN DE TU CAFÉ", font=("Arial", 16, "bold")).pack(pady=10)

# --- 3. RADIOBUTTONS (Tamaño) ---
tk.Label(ventana, text="1. Selecciona el TAMAÑO:", font=("Arial", 12)).pack()

# Creamos y enlazamos los Radiobuttons a la misma variable
tk.Radiobutton(ventana, text="Pequeño", variable=tamaño_seleccionado, value=1).pack(anchor="w")
tk.Radiobutton(ventana, text="Mediano", variable=tamaño_seleccionado, value=2).pack(anchor="w")
tk.Radiobutton(ventana, text="Grande", variable=tamaño_seleccionado, value=3).pack(anchor="w")

# --- 4. CHECKBUTTONS (Extras) ---
tk.Label(ventana, text="\n2. Selecciona EXTRAS (cero o más):", font=("Arial", 12)).pack()

# Creamos los Checkbuttons, cada uno con su propia variable
tk.Checkbutton(ventana, text="Leche", variable=extra_leche).pack(anchor="w")
tk.Checkbutton(ventana, text="Azúcar", variable=extra_azucar).pack(anchor="w")
tk.Checkbutton(ventana, text="Crema batida", variable=extra_crema).pack(anchor="w")

# --- 5. LISTBOX (Tipo de Grano) ---
tk.Label(ventana, text="\n3. Selecciona el TIPO DE GRANO:", font=("Arial", 12)).pack()

listbox_grano = tk.Listbox(ventana, height=4, selectmode=tk.MULTIPLE) # MULTIPLE permite varios

# Insertamos las opciones
tipos_de_grano = ["Arábica", "Robusta", "Mezcla de la Casa", "Descafeinado"]
for tipo in tipos_de_grano:
    listbox_grano.insert(END, tipo) # END indica insertar al final de la lista

listbox_grano.pack(pady=5)

# --- 6. BOTÓN DE PROCESAR PEDIDO ---
boton_pedido = tk.Button(ventana, 
                         text="Generar Pedido", 
                         command=procesar_pedido, 
                         font=("Arial", 14, "bold"), 
                         bg="orange", 
                         fg="white")
boton_pedido.pack(pady=20)

# --- 7. LABEL DE RESUMEN FINAL ---
label_resumen = tk.Label(ventana, 
                         text="Tu pedido aparecerá aquí.", 
                         font=("Arial", 13), 
                         height=3)
label_resumen.pack()

# 8. EJECUCIÓN DEL BUCLE PRINCIPAL
ventana.mainloop()
```

-----

## 4\. 🚀 Ejecución en VS Code

1.  Guarda el archivo `pedido_cafe.py`.

2.  Abre la **Terminal** en VS Code.

3.  Ejecuta el programa con:

    ```bash
    python pedido_cafe.py
    ```

### 💡 Puntos Clave para Explicar a los Alumnos

1.  **Variables de Control (`tk.IntVar`)**: Explique que los elementos de Tkinter no devuelven su valor automáticamente. Necesitan una **variable intermediaria** (`IntVar` para números, `StringVar` para texto) que guarde su estado.

      * **Radiobutton**: Todos comparten la misma variable (`tamaño_seleccionado`), pero cada uno tiene un `value` diferente (`1`, `2`, `3`).
      * **Checkbutton**: Cada uno tiene su propia variable (`extra_leche`, etc.) que vale `1` si está marcado o `0` si no lo está.

2.  **Selección Múltiple (`Listbox`)**: Muestre cómo la opción `selectmode=tk.MULTIPLE` permite al usuario seleccionar varios elementos. La función clave para obtener esos elementos es **`.curselection()`**, que devuelve los índices de los elementos elegidos.
