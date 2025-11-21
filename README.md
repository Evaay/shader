# Shaders

## Motivación

El objetivo de este proyecto era explorar la creación de patrones generativos complejos y dinámicos utilizando la eficiencia de los shaders de fragmentos (GLSL). He querido realizar cambios de color y trabajar con las variables de u_time y u_mouse. También he querido generar nuevas formas o efectos visuales.

## Videos
Shader1:
Shader2:
Shader3:
---

## Descripción del Desarrollo

Al ser la primera vez trabajando con shaders, he decidido empezar por la documentación del profesor e ir mirando los ejemplos que se encontraban en el github, a su vez, también he estado mirando la documentaicón de la página de The Book of Shaders, donde habían ejemplos bastante atractivos con los que probar. Para esta entrega se han realizado varias pruebas, pero al sobre pasar alguna los límites establecidos del archivo, se ha decidido dejar estos 3 ejemplos.

Shader 1: Túnel Rotatorio con Ruido Dinámico
  Para este shader se empleó un shader base con efecto túnel.
  Se implementó una matriz de rotación 2x2 que depende de u_time para aplicar una transformación geométrica a la coordenada de cada píxel, logrando que todo el patrón rotara de forma continua.
  Se combinaron funciones smoothstep() para definir los bordes y sin() para introducir un patrón de ruido o líneas oscilantes. El uso de la distancia radial (length) permitió que el patrón se hiciera más denso hacia los bordes.
  La función mix() se utilizó para interpolar entre dos colores (color_A y color_B), ambos definidos por funciones trigonométricas que varían con u_time, asegurando un degradado de color cíclico y animado.

Shader 2: Patrón Distorsionado con Control Interactivo
  Lo que se quería hacer con este patrón era implemetnar algo de interactividad con el usuario.
  Se introdujo una distorsión a las coordenadas $x$ y $y$ mediante la suma de un valor aleatorio (fract(sin(...))). Este valor fue modulado directamente por la variable u_mouse.x, permitiendo al usuario controlar la intensidad del efecto de flujo o vórtice en tiempo real.
  La fórmula de color se basó en una variable v que era inversamente proporcional a la distancia (length) al centro. Esto creó una alta frecuencia de cambio de color cerca del centro.

Shader 3: Patrón de Círculos Animado
  Se centró en la creación de patrones y diferenciación de comportamiento por celda.
  Se escalaron las coordenadas de la textura por un factor de 8.0, y se usaron floor() y fract() para aislar la coordenada de la celda (index_x, index_y) de la coordenada local dentro de la celda (coords_local).
  Se utilizó el índice de la celda (index_x + index_y) como desfase para la función sin() que controlaba el radio del círculo. Esto garantizó que cada círculo dentro del mosaico se expandiera y contrajera de forma independiente a lo largo del tiempo, rompiendo la uniformidad del patrón y creando la sensación de movimiento.
  La forma del círculo se definió con smoothstep aplicada al radio de la cuadrícula, mientras que el color se definió mediante la misma técnica de paleta cíclica del Shader 1, asegurando un resultado visual coherente.

---

## Fuentes Utilizadas

Las fuentes utilizadas han sido las siguientes:

- **The Book of Shaders**
- **Documentación del profe**

---
