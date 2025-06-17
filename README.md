# Informe del Proyecto ABP - Cálculo Vectorial

## Datos generales

- **Nombre del proyecto**:  
- **Integrantes** (Nombres y códigos):  
- **Aula**: Teoría 1.01  
- **Sala**: Sala 1 / 2 / 3  
- **Grupo**: Grupo X
- **Docente**: Nombre y Apellido  

---

## 1. Comprensión del problema 
- Describa claramente los datos del problema.
- Identifique claramente el objetivo de la pregunta.
- Describa las variables involucradas en el problema.

---

## 2. Desarrollo y aplicación de la solución 

### 2.1 Ejecución de la solución

- Describa la secuencia de pasos que siguieron.
- Explique cómo aplicaron los conceptos matemáticos del curso.
- Justifique cada paso de su razonamiento.

### 2.2 Uso de herramienta CAS

  import matplotlib.pyplot as plt
from matplotlib import animation, rc
rc('animation', html='html5')

def init_fig(x, t, ws, cost_ws):
    """Initialise figure"""
    fig, ax = plt.subplots(1, 1, figsize=(6,5))
    ax.plot(ws, cost_ws, 'r-', label='coste')
    ax.set_ylim([-0.5, 4.2])
    ax.set_xlim([1, 3.1])
    ax.set_xlabel('x', fontsize=15)
    #ax.set_ylabel('coste: $1/2 \sum |\hat{y}-y|^2$', fontsize=15)
    ax.set_ylabel('$f(x)$', fontsize=15)
    ax.grid(True)
    cost_text = ax.set_title('$f(x)$ {}'.format(0), fontsize=18)
    line1, = ax.plot([], [], 'k:', label='derivada en $x$')
    pc_dots, = ax.plot([], [], 'ko')
    ax.legend(loc=2)
    return fig, ax, line1, pc_dots, cost_text

def get_anim(fig, ax, line1, pc_dots, cost_text, weights):
    """Return animation function."""
    xs = np.linspace(0, 4, num=100)  # weight values
    def anim(i):
        """Animate step i"""
        if i == 0:
            return [line1, pc_dots, cost_text]
        (w, dw, cost) = weights[i-1]
        cost_text.set_text(f'$f(x_{{{i}}}) = {cost:.3f}$')
        ws, _, cs = zip(*weights[0:i])
        pc_dots.set_xdata(ws)
        pc_dots.set_ydata(cs)
        abline_values = [dw * (x-w) + cost for x in xs]
        line1.set_xdata(xs)
        line1.set_ydata(abline_values)
        return [line1, pc_dots, cost_text]
    return anim

def gradient(w, x, t): 
    return np.sum(x * (x*w - t))

def cost(y, t): 
  return (0.5*(t - y)**2).sum()

x = np.random.rand(20)
y = 2*x + (np.random.rand(20)-0.5)*0.5
ws = np.linspace(0, 4, num=100)  
cost_ws = np.vectorize(lambda w: cost(x*w, y))(ws)  
fig, ax, line1, pc_dots, cost_text = init_fig(x, y, ws, cost_ws)

def compute_anim(w = 1, lr=0.01):
    epochs = 49
    weights = [(w, gradient(w, x, y), cost(x*w, y))]
    for i in range(epochs):
        dw = gradient(w, x, y)
        w = w - lr*dw
        weights.append((w, dw, cost(x*w, y)))

    animate = get_anim(fig, ax, line1, pc_dots, cost_text, weights)
    anim = animation.FuncAnimation(fig, animate, frames=len(weights)+1, interval=200, blit=True)
    plt.close()
    return anim
    
anim = compute_anim()
anim
- Incluir aquí los gráficos elaborados con CAS (GeoGebra, Desmos, Matlab, etc.).
- Explicar el significado de cada gráfico en relación con la solución.

### 2.3 Explicación gráfica

- Explique claramente las gráficas utilizadas.
- Identifique y resalte los elementos más importantes de las gráficas.
- Utilice el puntero de la presentación para señalar los elementos clave (esto es para la exposición).

---

## 3. Análisis y discusión

- Describa las estrategias utilizadas para validar su respuesta.
- Muestre cómo modificaron los parámetros del problema.
- Explique cómo los cambios en los parámetros afectan los resultados.
- Atribuya correctamente los cambios observados a las modificaciones realizadas.

---

## 4. Conclusiones

- Resuma los aprendizajes principales del trabajo realizado.
- Mencione posibles extensiones o mejoras al trabajo.

---

## 5. Bibliografía

- Liste todas las fuentes bibliográficas consultadas (libros, artículos, páginas web).

---

## 6. Reflexión sobre trabajo en equipo *(opcional, para autoevaluación)*

- Comente brevemente cómo fue la colaboración dentro del grupo.
- ¿Cómo contribuyó cada integrante al desarrollo del proyecto?

