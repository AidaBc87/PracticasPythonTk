El ejercicio convertirá Pesos Mexicanos (MXN) a Dólares Estadounidenses (USD), usando un tipo de cambio fijo.

1. 🎯 Objetivo de la Práctica
Crear una aplicación GUI con Tkinter que:
1.	Pida al usuario ingresar una cantidad en Pesos Mexicanos usando un campo de texto (Entry).
2.	Al presionar un botón (Button), calcule la conversión.
3.	Muestre el resultado en un texto (Label).
(Nota: Usaremos un tipo de cambio fijo de $1 USD = 18.00 MXN para simplificar.)
________________________________________
2. ⚙️ Preparación en VS Code
Sigue los mismos pasos de configuración:
1.	Crea una carpeta nueva (ej. Conversor_Tkinter).
2.	Abre la carpeta en VS Code.
3.	Crea un nuevo archivo llamado conversor.py.
________________________________________
3. 🐍 El Código Paso a Paso con Entry, Button y Label
Copia y pega el siguiente código en tu archivo conversor.py.
Sección 1: Importar y Definir Lógica
Esta función es el "cerebro" de la aplicación.
Python
import tkinter as tk
from tkinter import messagebox # Módulo extra para mensajes emergentes

# 1. Tipo de Cambio Fijo (simplificación)
TIPO_CAMBIO = 18.00 # 1 Dólar = 18.00 Pesos

# 2. Función que maneja el evento del botón
def convertir_moneda():
    try:
        # 2.1. Obtener el texto del Entry
        pesos_str = entrada_pesos.get()
        
        # 2.2. Convertir el texto a número decimal (flotante)
        pesos_mxn = float(pesos_str)
        
        # 2.3. Realizar la conversión
        dolares_usd = pesos_mxn / TIPO_CAMBIO
        
        # 2.4. Actualizar el texto del Label de resultado
        # f-string formatea el resultado a dos decimales
        resultado_label.config(text=f"Son: $ {dolares_usd:.2f} USD 💵")
        
    except ValueError:
        # Manejo de error si el usuario no ingresa un número válido
        messagebox.showerror("Error de Entrada", "Por favor, introduce una cantidad numérica válida.")
        # Opcional: limpiar el campo de entrada
        entrada_pesos.delete(0, tk.END)
Concepto Clave	Explicación
entrada_pesos.get()	Método para extraer el texto que el usuario escribió en el casilla de entrada.
float(pesos_str)	Convierte la cadena de texto obtenida del Entry a un número decimal (float) para poder realizar la división.
try...except ValueError	Un mecanismo de Python para manejar errores. Si el usuario escribe "hola" en lugar de un número, el programa no se detiene, sino que muestra un mensaje de error.
resultado_label.config()	Cambia la propiedad text de la etiqueta para mostrar el resultado.
________________________________________
Sección 2: Configurar la Ventana y Widgets
Aquí definimos los elementos visuales (Entry, Label, Button) y los colocamos en la ventana.
Python
# 3. CREAR LA VENTANA PRINCIPAL
ventana = tk.Tk()
ventana.title("Conversor MXN a USD 💲")
ventana.geometry("400x200")
ventana.resizable(False, False)

# --- CREACIÓN DE WIDGETS ---

# 4. LABEL de Instrucción
label_instruccion = tk.Label(ventana, 
                             text="Introduce la cantidad en Pesos Mexicanos (MXN):", 
                             font=("Arial", 10))

# 5. ENTRY (Casilla de Entrada de Texto)
entrada_pesos = tk.Entry(ventana, 
                         width=15, 
                         font=("Arial", 12), 
                         justify='center') # El texto aparece centrado

# 6. BUTTON (Botón)
boton_convertir = tk.Button(ventana, 
                            text="CONVERTIR A USD", 
                            command=convertir_moneda, # Enlace a la función
                            font=("Arial", 11), 
                            bg="lightblue")

# 7. LABEL de Resultado (Inicialmente vacío)
resultado_label = tk.Label(ventana, 
                           text="Esperando cantidad...", 
                           font=("Arial", 14, "bold"), 
                           fg="green")

# --- POSICIONAMIENTO (usando .pack) ---

label_instruccion.pack(pady=(20, 5)) # Padding superior e inferior
entrada_pesos.pack(pady=5)
boton_convertir.pack(pady=10)
resultado_label.pack(pady=15)

# 8. EJECUTAR EL BUCLE PRINCIPAL
ventana.mainloop()
________________________________________
4. 🚀 Ejecución en VS Code
1.	Guarda el archivo conversor.py (Ctrl + S).
2.	Abre la Terminal en VS Code (Ctrl + Shift + Ñ).
3.	Ejecuta el programa con el comando:
Bash
python conversor.py
✅ Prueba del Programa
•	Si ingresas 180: Al presionar CONVERTIR A USD, el Label de resultado cambiará a: Son: $ 10.00 USD 💵.
•	Si ingresas xyz: Aparecerá el mensaje de error: "Por favor, introduce una cantidad numérica válida."
¡Con esta práctica has usado los tres controles fundamentales de Tkinter!
