# Apuntes_U2
Esta unidad explora los fundamentos matemáticos y algorítmicos detrás de la generación y manipulación de imágenes en dos dimensiones, desde transformaciones básicas hasta la creación de formas complejas y fractales.
# 2.1 Transformacion bidimencional
Las transformaciones permiten alterar la posición, el tamaño o la orientación de un objeto en el plano $XY$.
# 2.1.1 Traslación
Mueve un objeto a una nueva posición sumando desplazamientos $(t_x, t_y)$ a sus coordenadas originales.
* Ecuación: $x' = x + t_x$, $y' = y + t_y$.
# 2.1.2 Escalamiento
Cambia el tamaño de un objeto multiplicando sus coordenadas por factores de escala $(s_x, s_y)$.
* Ecuación: $x' = x \cdot s_x$, $y' = y \cdot s_y$.
# 2.1.3 Rotación
Gira un objeto alrededor de un punto pivote (normalmente el origen) con un ángulo $\theta$.
* Ecuación: * $x' = x \cos\theta - y \sin\theta$
* $y' = x \sin\theta + y \cos\theta$
# 2.1.4 Sesgado (Shear)
Distorsiona la forma de un objeto desplazando sus puntos de forma proporcional a su distancia a un eje.
* Ejemplo en X: $x' = x + sh_x \cdot y$.
# 2.2 Representación Matricial
Para optimizar procesos, las transformaciones se representan mediante matrices de $3 \times 3$ usando coordenadas homogéneas. Esto permite realizar múltiples transformaciones (composición) con una sola multiplicación de matrices.
## Implementación Práctica (Script de Blender)
El siguiente código muestra cómo aplicar una Traslación en tiempo real vinculada a eventos del teclado (flechas de dirección) dentro de Blender:
``` python
import bpy

class ModalMoveOperator(bpy.types.Operator):
    """Controlar objeto con flechas del teclado"""
    bl_idname = "object.modal_move_operator"
    bl_label = "Modal Move Operator"

    def modal(self, context, event):
        obj = bpy.data.objects.get("Stroke")
        if obj is None: return {'RUNNING_MODAL'}

        # Lógica de Traslación (sección 2.1.1)
        if event.type == 'LEFT_ARROW' and event.value == 'PRESS':
            obj.location.x -= 0.5
        elif event.type == 'RIGHT_ARROW' and event.value == 'PRESS':
            obj.location.x += 0.5
        elif event.type == 'UP_ARROW' and event.value == 'PRESS':
            obj.location.z += 0.5 # Mapeo de verticalidad en 3D
        elif event.type == 'DOWN_ARROW' and event.value == 'PRESS':
            obj.location.z -= 0.5
        elif event.type == 'ESC':
            return {'FINISHED'}

        return {'RUNNING_MODAL'}

    def invoke(self, context, event):
        context.window_manager.modal_handler_add(self)
        return {'RUNNING_MODAL'}

bpy.utils.register_class(ModalMoveOperator)
``` 
# 2.3 Trazo de Líneas Curvas
A diferencia de los vectores rectos, las curvas se definen mediante funciones polinomiales basadas en puntos de control.
# 2.3.1 Bézier
Utilizadas ampliamente en diseño gráfico. La curva es atraída por los puntos de control intermedios, pero solo pasa por el primero y el último.
# 2.3.2 B-spline
Ofrecen un control más fino que las Bézier. Un cambio en un punto de control solo afecta a una sección local de la curva, lo que facilita el modelado de personajes complejos.
## Desarrollo del personaje
* Uso de Curvas: Los contornos del conejo se definieron mediante puntos de control (Bézier), permitiendo una resolución infinita y bordes suavizados, característicos de la graficación 2D vectorial.
* Interpolación: La animación utiliza la interpolación entre fotogramas clave (keyframes), donde el software calcula matemáticamente la posición intermedia del objeto siguiendo una trayectoria curva.
## Implementación del Control
Como se vio en la Sección 2.2, el movimiento del personaje no es aleatorio, sino que responde a una matriz de transformación que cambia según la interacción (como el script de Python para las flechas del teclado).
<img width="563" height="342" alt="image" src="https://github.com/user-attachments/assets/94e39d74-210d-4bcf-8f6b-6ad7224dee41" />

# 2.4 Fractales
Un fractal es un objeto cuya estructura básica se repite a diferentes escalas. En la graficación moderna, se usan para generar texturas naturales o paisajes en la animación.
* Autosimilitud: Si hiciéramos un acercamiento infinito a una parte del dibujo que posea propiedades fractales, veríamos la misma forma original.
# 2.5 Uso y creación de fuentes de texto
Finalmente, para los títulos o créditos de la animación, se utilizan fuentes de contorno (Outline Fonts). Estas fuentes guardan la información de cada letra como un conjunto de curvas matemáticas (Bézier), lo que permite escalarlas en Blender sin que se vean "pixeleadas".
# Bibliografias
* Foley, J. D., Van Dam, A., Feiner, S. K., & Hughes, J. F. (1996). Introducción a la graficación por computador. Addison-Wesley Iberoamericana.
* Hearn, D., & Baker, M. P. (2006). Gráficas por computadora con OpenGL (3ra ed.). Pearson Educación.
* Joyanes Aguilar, L. (2008). Programación en C++, Java y C#. Algoritmos, estructuras de datos y objetos. McGraw-Hill.
* Salomón, D. (2006). Curvas y superficies para diseño asistido por computadora. Paraninfo.
* Watt, A. (1995). 3D Computer Graphics (Trad. al español: Gráficos por computadora). Addison-Wesley.
