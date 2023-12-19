- 👋 HOLA SOY ARMANDO
- 👀 ULTIMAMENTE E ESTADO HACIENDO UN CODIGO PARA UNA IA QUE PUEDA DIBUJAR Y HACER VIDEOS UTILIZANDO SPACY Y WEBDRIVER
- ME GUSTARIA SI PUDIERAN AYUDARME A DECIRME SI LO ESTOY HACIENDO BIEN, SOY NUEVO EN ESTO DE LA PROGRAMACION Y E ESTADO SIENDO AYUDADO POR LA IA DE CHAT GPT
- LO QUE QUIERO HACER SON 3 ARCHIVOS DE PROGRAMA.PY EN LOS CUALES SE CREE UNA PERSONA, ES DECIR, UNO VIENE SIENDO EL CENTRAL EL CUAL SE LLAMA EXANOVA, EL SECUNDARIO SE LLAMA IMAGINACION Y EL TERCIARIO SE LLAMA ASISTENTE.
- EL PRINCIPAL ES EL QUE TIENE TODA LA INFO DE EL COMO SE PUEDE VIAJAR POR INTERNET Y MIRAR FOTOS EN INTERNET, Y LOS SECUNDARIOS SON IMAGINACION PARA VER IMAGENES EN INTERNET Y USAQRLS PARA CREAR UNA BASADA EN LAS PREVIAMENTE VISTAS
import os
import cv2
import torch
import pyttsx3
import threading
import time
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from imaginacion import Creatividad
from PIL import Image

# Clase Voz
class Exanova:
    def __init__(self):
        # Configurar el controlador de Edge
        self.edge_driver_path = 'C:/Program Files (x86)/Microsoft/Edge/Application/msedgedriver.exe'
        self.driver = webdriver.Edge(executable_path=self.edge_driver_path)

        # Configurar el motor de síntesis de voz
        self.engine = pyttsx3.init()

    def buscar_en_web(self, consulta):
        # Utilizar el controlador de Edge para realizar una búsqueda en la web
        self.driver.get(f"https://www.bing.com/search?q={consulta}")

        # Obtener los títulos de los resultados y convertirlos a voz
        resultados = self.driver.find_elements_by_css_selector('h2')
        textos_resultados = [resultado.text for resultado in resultados]
        for texto in textos_resultados:
            self.decir(texto)

    def decir(self, texto):
        # Utilizar el motor de síntesis de voz para decir el texto
        self.engine.say(texto)
        self.engine.runAndWait()

def capturar_pantalla(asistente, creatividad):
    while True:
        if not busqueda_en_progreso:
            # Capturar la pantalla y guardarla como "captura.png"
            screenshot_path = "captura.png"
            os.system("screencapture -x " + screenshot_path)

            # Analizar y combinar las imágenes usando imaginacion
            creatividad.analizar_y_combinar("imagen1.jpg", screenshot_path, "resultado.png")

            # Dibujar la imagen resultante con imaginacion
            creatividad.dibujar_imagen("resultado.png")

            # Limpiar la captura
            os.remove(screenshot_path)

            # Esperar antes de la próxima captura
            time.sleep(5)

if __name__ == "__main__":
    # Inicializar variables
    busqueda_en_progreso = False

    # Crear una instancia de la clase Voz y Creatividad
    mi_voz = Exanova()
    mi_creatividad = Creatividad()

    # Crear un hilo para la captura de pantalla
    hilo_captura_pantalla = threading.Thread(target=capturar_pantalla, args=(mi_voz, mi_creatividad))
    hilo_captura_pantalla.start()

    try:
        # Ejemplo de cómo utilizar la instancia para buscar en la web y convertir los resultados a voz
        mi_voz.buscar_en_web("machine learning")

        # Esperar a que el hilo de captura de pantalla termine
        hilo_captura_pantalla.join()

    except KeyboardInterrupt:
        # Detener el hilo de captura de pantalla al presionar Ctrl+C
        hilo_captura_pantalla.join()
        print("Programa detenido por el usuario.")
este es mi codigo principal de exanova :3

#imaginacion, programa secundario
from Exanova import os, cv2, torch, F, plt, np, Image, hablar, escuchar


def funcion_imaginacion():
    from asistente import funcion_asistente
    

class Imaginacion:
    def __init__(self, voz_instancia):
        self.voz_instancia = voz_instancia
        self.ruta_ejemplos = "creatividad"  # Cambiado de "ejemplos" a "creatividad"
        self.contenido_anterior = set()

    def dibujar_en_2d(self, imagen, depth_of_field):
        intensidad = imagen.mean(axis=2)
        alto, ancho = intensidad.shape
        x = torch.linspace(0, ancho - 1, ancho)
        y = torch.linspace(0, alto - 1, alto)
        x, y = torch.meshgrid(x, y)
        x, y, intensidad = x.flatten(), y.flatten(), intensidad.flatten()

        # Aplicar variación sinusoidal en la profundidad de campo
        intensidad = intensidad + 5.0 * torch.sin(2 * np.pi * x / 100.0) * torch.sin(2 * np.pi * y / 100.0)

        plt.scatter(x.numpy(), y.numpy(), c=intensidad.numpy(), cmap='gray', marker='.')
        plt.xlabel('X')
        plt.ylabel('Y')
        plt.title('Representación 2D de la Imagen con Profundidad de Campo')
        plt.show()

    def dibujar_en_3d(self, imagen, depth_of_field):
        alto, ancho, _ = imagen.shape
        x = torch.linspace(0, ancho, ancho)
        y = torch.linspace(0, alto, alto)
        x, y = torch.meshgrid(x, y)
        intensidad = imagen.mean(axis=2)
        x, y, intensidad = x.flatten(), y.flatten(), intensidad.flatten()

        fig = plt.figure()
        ax = fig.add_subplot(111, projection='3d')

        # Aplicar variación sinusoidal en la profundidad de campo
        intensidad = intensidad + 5.0 * torch.sin(2 * np.pi * x / 100.0) * torch.sin(2 * np.pi * y / 100.0)

        ax.scatter(x.numpy(), y.numpy(), intensidad.numpy(), c=intensidad.numpy(), cmap='gray')

        ax.set_xlabel('X')
        ax.set_ylabel('Y')
        ax.set_zlabel('Intensidad')
        ax.set_title('Representación 3D de la Imagen con Profundidad de Campo')

        plt.show()

    def dibujar_pixeleado(self, imagen):
        ancho = imagen.shape[1]
        alto = imagen.shape[0]
        tamaño_pixel = 10
        imagen_pixeleada = F.avg_pool2d(imagen, kernel_size=tamaño_pixel, stride=tamaño_pixel)

        fig, ax = plt.subplots()
        ax.imshow(imagen_pixeleada.numpy(), cmap='gray', vmin=0, vmax=1)
        ax.axis('off')
        ax.set_title('Dibujo Pixeleado')
        plt.show()

    def crear_video(self):
        video_writer = cv2.VideoWriter('video_salida.avi', cv2.VideoWriter_fourcc(*'XVID'), 20.0, (640, 480))

        for i in range(200):
            frame = torch.zeros((480, 640, 3), dtype=torch.uint8).numpy()

            if self.puede_dibujar:
                depth_of_field = 10.0 + 5.0 * np.sin(2 * np.pi * i / 100.0)
                self.dibujar_en_2d(frame, depth_of_field)
                self.dibujar_en_3d(frame, depth_of_field)

            video_writer.write(frame)

        video_writer.release()

    def analizar_ejemplos(self):
        ejemplos = self.obtener_ejemplos()
        for ejemplo in ejemplos:
            if ejemplo.endswith(".jpg") or ejemplo.endswith(".png"):
                self.analizar_imagen(os.path.join(self.ruta_ejemplos, ejemplo))
            elif ejemplo.endswith(".mp4"):
                self.analizar_video(os.path.join(self.ruta_ejemplos, ejemplo))

    def obtener_ejemplos(self):
        return os.listdir(self.ruta_ejemplos)

    def analizar_imagen(self, ruta_imagen):
        imagen = cv2.imread(ruta_imagen)
        if imagen is not None:
         nueva_imagen = self.crear_representacion(imagen)
        cv2.imwrite(f"nueva_{os.path.basename(ruta_imagen)}", nueva_imagen)

    def analizar_video(self, ruta_video):
        pass

    def crear_representacion(self, imagen):
        imagen = torch.tensor(imagen).permute(2, 0, 1).unsqueeze(0).float() / 255.0
        imagen_pixeleada = F.avg_pool2d(imagen, kernel_size=10, stride=10)
        nueva_imagen = (imagen_pixeleada[0] * 255.0).byte().permute(1, 2, 0).numpy()
        return nueva_imagen

    def guardar_imagen_creada(self, imagenes, videos):
        for imagen in imagenes:
            ruta_imagen = os.path.join(self.ruta_ejemplos, imagen)
            nueva_imagen = self.analizar_imagen(ruta_imagen)
            cv2.imwrite(f"nueva_{os.path.basename(ruta_imagen)}", nueva_imagen)

        for video in videos:
            ruta_video = os.path.join(self.ruta_ejemplos, video)
            pass

    def revisar_nuevo_contenido(self):
        contenido_actual = set(os.listdir(self.ruta_ejemplos))

        if contenido_actual:
            nuevo_contenido = contenido_actual - self.contenido_anterior
            if nuevo_contenido:
                print(f"¡Nuevo contenido encontrado en 'creatividad'! {nuevo_contenido}")
                self.preguntar_realizar_dibujo()

        self.contenido_anterior = contenido_actual

# imaginacion.py

def funcion_imaginacion():
    from asistente import funcion_asistente
    # Resto del código de la función



# Uso de la clase Creatividad
Imaginacion = Imaginacion(voz_instancia=None)  # Reemplaza None con tu instancia de Exanova
Imaginacion.guardar_imagen_creada(imagenes=["imagen1.jpg", "imagen2.png"], videos=["video1.mp4"])

# Uso de la clase Creatividad
Imaginacion = Imaginacion(voz_instancia=None)  # Reemplaza None con tu instancia de Exanova
Imaginacion.analizar_ejemplos()

este el de imaginacion :3


from Exanova import pyttsx3, sr, datetime, abrir_microsoft_edge_con_url
from imaginacion import Imaginacion
nombre = "Armando"
# Inicializar Creatividad
creatividad = Creatividad()

def funcion_asistente():
    if "abrir Microsoft Edge" in comando:
        abrir_microsoft_edge()

def funcion_imaginacion():
    # Resto del código de la función imaginacion
    pass

def hablar(texto):
    motor_tts = pyttsx3.init()
    motor_tts.setProperty('rate', 150)
    motor_tts.setProperty('pitch', 150)
    motor_tts.say(texto)
    motor_tts.runAndWait()
    motor_tts.stop()

def escuchar():
    reconocedor = sr.Recognizer()
    texto = ""

    with sr.Microphone() as source:
        print("Escuchando...")
        reconocedor.adjust_for_ambient_noise(source)
        audio = reconocedor.listen(source)

    try:
        print("Reconociendo...")
        texto = reconocedor.recognize_google(audio, language="es-ES")
        print("{nombre}:", texto)
        return texto
    except sr.UnknownValueError:
        print("No se pudo entender el audio.")
    except sr.RequestError as e:
        print(f"No te logré entender, ¿podrías repetirlo?: {e}")

def obtener_bienvenida():
    hora_actual = datetime.datetime.now().hour
    if 6 <= hora_actual < 12:
        return f"Buenos días, {nombre}. ¿En qué puedo ayudarte?"
    elif 12 <= hora_actual < 18:
        return f"Buenas tardes, {nombre}. ¿Cómo puedo ayudarte?"
    else:
        return f"Buenas noches, {nombre}. ¿En qué puedo ayudarte?"

def tarea_principal():
    while ejecucion:
        comando = escuchar()

        if comando:
            hablar(obtener_bienvenida())
            procesar_comando(comando)

def procesar_comando(comando):
    for clave, valor in comandos.items():
        if isinstance(valor, list):
            if any(opcion in comando for opcion in valor):
                valor()  # Llama a la función correspondiente
                hablar(f"Ejecutando comando: {clave}")
                break
        elif valor in comando:
            valor()  # Llama a la función correspondiente
            hablar(f"Ejecutando comando: {clave}")
            break
    else:
        hablar("Perdón, ¿qué dijiste?")

def buscar_en_microsoft_edge():
    url = "https://www.microsoft.com"
    abrir_microsoft_edge_con_url(url)

def buscar_musica_en_microsoft_edge():
    if "abre youtube music" in comando:
        url = "https://music.youtube.com/"
        abrir_microsoft_edge_con_url(url)

def buscar_clima_en_microsoft_edge():
    if "buscar clima" in comando:
        url = "https://www.bing.com/search?pglt=171&q=clima+en+tijuana&cvid=b9917b218cad40dba1254148e47d0710&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQABhAMgYIAhAAGEAyBggDEAAYQDIGCAQQABhAMgYIBRAAGEAyBggGEAAYQDIGCAcQABhAMgYICBAAGEDSAQgzNDQxajBqMagCALACAA&FORM=ANNTA1&PC=VALBAN"
        abrir_microsoft_edge_con_url(url)

def buscar_noticias_en_microsoft_edge():
    if "buscar noticias" in comando:
        url = "https://www.bing.com/news"
        abrir_microsoft_edge_con_url(url)

def abrir_microsoft_edge():
    hablar("Okay hermanito.")
    buscar_en_microsoft_edge()

def buscar_noticias():
    hablar("Y las noticias de hoy son... estas. ¡Jajaja!")
    buscar_noticias_en_microsoft_edge()

def buscar_musica():
    hablar("Música, maestro... O debería decir maestra. Solo bromeo contigo. ¡Jajaja!")
    buscar_musica_en_microsoft_edge()

comandos = {
    "abrir Microsoft Edge": abrir_microsoft_edge,
    "buscar noticias": buscar_noticias,
    "buscar música": buscar_musica,
}

if __name__ == "__main__":
    asistente = Asistente()

    while True:
        comando = escuchar()
        procesar_comando(comando)
y este el de asistente :3 
