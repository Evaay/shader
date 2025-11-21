# Shaders

## 1. Motivación

El objetivo de este proyecto era explorar la creación de patrones generativos complejos y dinámicos utilizando la eficiencia de los shaders de fragmentos (GLSL).

**Exploración visual:**  
Diseñar un shader que combinara movimiento armónico (pulsos de color) con estructura modular (una rejilla) para simular un efecto de túnel o vórtices celulares hipnóticos.

---

## 2. Descripción Detallada del Desarrollo

El shader está construido sobre dos capas principales:

- La capa de **estructura** (el patrón en cuadrícula).
- La capa de **color** (el gradiente animado por tiempo).

---

### 2.1. Creación de la Estructura Modular (Rejilla 8×8)

El patrón comienza dividiendo la pantalla en una rejilla de **8×8** y aislando las coordenadas dentro de cada celda:

- **Escalado y Rejilla:**  
  Las coordenadas normalizadas `st` se multiplican por `8.0`.

- **Identificación de celda:**  
  Las variables `x` y `y` se obtienen con:

  ```glsl
  floor(st)
  ```

  para identificar la columna y la fila actual.

- **Coordenadas locales:**  
  La función:

  ```glsl
  fract(st)
  ```

  devuelve coordenadas dentro del rango `[0.0, 1.0]` permitiendo dibujar el mismo patrón repetido en cada celda.

---

### 2.2. Generación del Patrón Circular Pulsante

Dentro de cada celda, el patrón se basa en:

- La distancia `r` al centro de la celda.
- El factor de intensidad `c`.

**Centrado:**

```glsl
s = s - 0.5;
```

**Radio:**

```glsl
r = length(s);
```

**Borde animado:**

El borde se define con:

```glsl
c = smoothstep(0.4 + a, 0.6 + a, r);
```

donde:

```glsl
a = sin(t + x + y) * 0.1;
```

Esto provoca que los círculos crezcan y se contraigan suavemente de forma independiente en cada celda.

---

### 2.3. Aplicación de Color Dinámico

Se define una paleta de dos colores (`c1` y `c2`) que varían continuamente mediante funciones trigonométricas como:

```glsl
sin(t * 0.5), cos(t * 0.3)
```

La mezcla final se obtiene mediante:

```glsl
vec3 final_color = mix(c1, c2, c);
```

El color base proviene de `c1`, mientras que `c2` define el tono del patrón circular.

---

## 3. Versión Tiny Code (Menos de 512 Bytes)

A continuación se adjunta la versión altamente comprimida del shader, sin funciones auxiliares ni variables descriptivas:

```glsl
void main(){
vec2 s=gl_FragCoord.xy/u_resolution;
float t=u_time;
s*=8.;
float x=floor(s.x),y=floor(s.y);
s=fract(s)-.5;
float r=length(s),a=sin(t+x+y)*.1;
float c=smoothstep(.4+a,.6+a,r);
vec3 c1=vec3(sin(t*.5),cos(t*.3),sin(t*.7))*.5+.5;
vec3 c2=vec3(cos(t*.4),sin(t*.6),cos(t*.2))*.5+.5;
gl_FragColor=vec4(mix(c1,c2,c),1.);
}
```

---

## 4. Fuentes Utilizadas 📚

El desarrollo del shader se apoya en:

- **The Book of Shaders**:  
  Conceptos de coordenadas, funciones (`fract`, `floor`), y `smoothstep`.

- **Patrones Modulares y Repetición:**  
  Técnicas estándar en programación de shaders basadas en el uso de `floor` y `fract` para subdividir el espacio visual.

- **Generación de Paletas Dinámicas:**  
  Inspirado en métodos de Inigo Quilez para crear gradientes animados con funciones trigonométricas.

---
