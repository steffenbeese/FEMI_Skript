---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.7
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

+++ {"editable": true, "slideshow": {"slide_type": "skip"}}


+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

# Ansatzfunktionen

Um verzerrungsfreie Starrkörperbewegungen sowie konstante Verzerrungszustände beschreiben zu können, und dabei ein räumlich isotropes Verformungsverhalten zu approximieren, müssen die Ansatzfunktionen vollständig sein. 
Für ein ebenes Dreieckselement bedeutet dies z.B.
```{math}	
:label: ansatzfunktionenCompleteTriangle
\begin{align}
u(\xi,\eta) &= \underbrace{\underbrace{ a_1 + a_2 \xi + a_3 \eta}_{\text{3 Knoten Dreieck}} + a_4 \xi^2 + a_5 \eta^2 + a_6 \xi \eta}_{\text{6 Knoten Dreieck}} \; .
\end{align}
```
Hier sind für ein 3-Knoten-Dreieck Element alle linearen Terme enthalten, für ein 6-Knoten-Dreieck Element sind auch die quadratischen Terme und bilinearen Terme enthalten. 
Illustriert kann man sich am besten ein pascalsches Dreieck wie in Abbildung {numref}`pascaltriangle` vorstellen.

```{figure} images/Taylor_PascalTriangle.png
---
name: pascaltriangle
alt: Pascalsches Dreieck für Dreieckselemente
width: 45%
---
Pascal'sches Dreieck für Dreieckselemte ({cite}`zienkiewicz2005finite`)
```

Analog kann dazu man ein vollständiges Viereckselement definieren:
```{math}		
:label: ansatzfunktionenCompleteQuad
\begin{align}
u(\xi,\eta) &= \underbrace{\underbrace{ a_1 + a_2 \xi + a_3 \eta + a_4 \xi \eta}_{\text{4 Knoten Viereck}} + a_5 \xi^2 + a_6 \eta^2 + a_7 \xi^2 \eta + a_8 \xi \eta^2}_{\text{8 Knoten Viereck}} \; .
\end{align}
```

Auch hier liefert das Pascalsche Dreieck eine anschauliche Interpretation:

```{figure} images/Taylor_PascalQuad.png
---
name: pascalQuad
alt: Pascalsches Dreieck für Rechteckelemente
width: 45%
---
Pascalsches Dreieck für Rechteckelemente ({cite}`zienkiewicz2005finite`). Grau unterlegt ist ein kubisches Rechteckelement. 
```

## Formfunktionen für $C^0$-Elemente

Nachfolgend werden kurz die Ansatzfunktionen für 1D, 2D und 3D Elemente mit Lagrange-Polynomen vorgestellt:

### 1D Formfunktionen

```{figure} images/Line-Elements.png
---
name: linearElements
alt: LineElements
width: 500px
---
1D-Elemente mit linearer und quadratischer Ansatzfunktion 
```

Für den linearen Fall haben 1D-Elemente 2 Knoten und damit 2 Formfunktionen:

```{math}
:label: 1D_Formfunktionen
\begin{align}
N_1 &= \frac{1}{2} (1 - \xi) & N_2 &= \frac{1}{2} (1 + \xi)
\end{align}
```

Für den quadratischen Fall haben 1D-Elemente 3 Knoten und damit 3 Formfunktionen:
```{math}
:label: 1D_Formfunktionen_quad 
\begin{align}
N_1 &= \frac{1}{2} \xi (\xi - 1) & N_2 &= 1 - \xi^2 & N_3 &= \frac{1}{2} \xi (\xi + 1)
\end{align}
```


<!-- ```{figure} images/Shapefunctions_1d.png
---
name: Shapefunctions_1dd
alt: Formfunktionen für 1D-Elemente
width: 500px
---
Formfunktionen für 1D-Elemente
``` -->


```{code-cell} ipython3
---
mystnb:
  figure:
    caption: |
      Formfunktionen für 1D-Elemente
    name: Shapefunctions_1dd
tags: [
  "hide-input"
  ]
---
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import Circle
import scienceplots

# Set up the plot configurations.
plt.rcParams['lines.linewidth'] = 2.0
plt.rcParams['lines.color'] = 'black'
plt.rcParams['legend.frameon'] = True
plt.rcParams['figure.figsize'] = (8, 6)
plt.rcParams['font.family'] = 'serif'
plt.rcParams['legend.fontsize'] = 15
plt.rcParams['font.size'] = 15
plt.rcParams['axes.spines.right'] = False
plt.rcParams['axes.spines.top'] = False
plt.rcParams['axes.spines.left'] = True
plt.rcParams['axes.spines.bottom'] = True
plt.rcParams['axes.axisbelow'] = True
plt.rcParams['grid.color'] = 'grey'
plt.rcParams['grid.alpha'] = 0.6
plt.rcParams['grid.linestyle'] = '--'

xi = np.linspace(-1, 1, 100)

# Create a figure and axis.
fig, ax = plt.subplots(1, 2, figsize=(12, 6))

# Plotting the functions.
N11 = 0.5*(1-xi)
N12 = 0.5*(1+xi)
ax[0].plot(xi,N11)
ax[0].plot(xi,N12)

N21 = 0.5*xi*(xi-1)
N22 = 0.5*xi*(xi+1) 
N23 = 1-xi**2 

ax[1].plot(xi,N21)
ax[1].plot(xi,N22)
ax[1].plot(xi,N23)


# Draw black line from (0,0) to (1,0)
ax[0].plot([-1, 1], [0, 0], color='black', linewidth=5.0)
ax[1].plot([-1, 1], [0, 0], color='black', linewidth=5.0)


# Draw red dots at (0,0) and (1,0)
ax[0].plot(-1, 0, 'ro', markersize=10)
ax[0].plot(1, 0, 'ro', markersize=10)

ax[1].plot(-1, 0, 'ro', markersize=10)
ax[1].plot(1, 0, 'ro', markersize=10)
ax[1].plot(0, 0, 'ro', markersize=10)


# Annotate the red dots.
# Draw a circle and add text at the specified location
circle = Circle((-1.1, 0.0), 0.1, color='red', fill=False,linewidth=2)
circle2 = Circle((1.1, 0.0), 0.1, color='red', fill=False,linewidth=2)

circle21 = Circle((-1.1, 0.0), 0.1, color='red', fill=False,linewidth=2)
circle22 = Circle((1.1, 0.0), 0.1, color='red', fill=False,linewidth=2)
circle23 = Circle((0, -0.2), 0.1, color='red', fill=False,linewidth=2)

ax[0].add_patch(circle)
ax[0].add_patch(circle2)
ax[0].annotate('1', xy=(-1, 0), xytext=(-1.15, -0.04), color='red')
ax[0].annotate('2', xy=(1, 0), xytext=(1.05, -0.04), color='red')

ax[1].add_patch(circle21)
ax[1].add_patch(circle22)
ax[1].add_patch(circle23)
ax[1].annotate('1', xy=(-1, 0), xytext=(-1.15, -0.04), color='red')
ax[1].annotate('2', xy=(1, 0), xytext=(1.05, -0.04), color='red')
ax[1].annotate('3', xy=(0, -0.2), xytext=(-0.04, -0.24), color='red')


# Set labels for the axis
ax[0].set_xlabel(r"$\xi$")
ax[0].set_ylabel(r"$N(\xi)$")
ax[1].set_xlabel(r"$\xi$")
ax[1].set_ylabel(r"$N(\xi)$")

# Draw background grid
ax[0].grid(True)
ax[1].grid(True)

# Add legend.
ax[0].legend([r"$N_1(\xi)$", r"$N_2(\xi)$"], loc='upper center', bbox_to_anchor=(0.5, -0.3), ncol=2)
ax[0].set_aspect('equal')

ax[1].legend([r"$N_1(\xi)$", r"$N_2(\xi)$", r"$N_3(\xi)$"], loc='upper center', bbox_to_anchor=(0.5, -0.3), ncol=3)
ax[1].set_aspect('equal')

```

### 2D Formfunktionen

```{figure} images/QuadElements.png
---
name: quadElements
alt: QuadElements
width: 500px
---
2D-Rechteck-Elemente mit linearer und quadratischer Ansatzfunktion 
```

Im 2D-Fall werden die Formfunktionen für Rechteck-Elemente als Tensorprodukt der 1D-Formfunktionen definiert. Für ein 2D-Rechteck-Element mit 4 Knoten (lineares Element) sind die Formfunktionen:

```{math}	
:label: 2D_Formfunktionen_Q4
\begin{align}
N_1 &= \frac{1}{4} (1 - \xi)(1 - \eta) & N_2 &= \frac{1}{4} (1 + \xi)(1 - \eta) \\
N_3 &= \frac{1}{4} (1 + \xi)(1 + \eta) & N_4 &= \frac{1}{4} (1 - \xi)(1 + \eta)
\end{align}
```



```{figure} images/shape_functions_Q4.png
---
name: quadElementsQ4N
alt: QuadElements
width: 500px
---
Bilineare Formfunktionen für 2D-Rechteck-Elemente.
```

Um ein vollständiges Element der Ordnung 2 zu erhalten benötigt man 9 Knoten beim Rechteck-Element. Die entsprechenden Formfunktionen lauten:

```{math}	
:label: 2D_Formfunktionen_Q9
\begin{align*}
N_1 &= \frac{1}{4} \xi \eta (\xi - 1)(\eta - 1) & N_2 &= \frac{1}{2} (1 - \xi^2) (1 - \eta) \\
N_3 &= \frac{1}{4} \xi \eta (\xi + 1)(\eta - 1) & N_4 &= \frac{1}{2} (1 + \xi) (1 - \eta^2) \\
N_5 &= \frac{1}{4} \xi \eta (\xi + 1)(\eta + 1) & N_6 &= \frac{1}{2} (1 - \xi^2) (1 + \eta) \\
N_7 &= \frac{1}{4} \xi \eta (\xi - 1)(\eta + 1) & N_8 &= \frac{1}{2} (1 - \xi) (1 - \eta^2) \\
N_9 &= (1 - \xi^2)(1 - \eta^2)
\end{align*}
```


```{figure} images/shape_functions_Q9.png
---
name: quadElementsQ9N
alt: QuadElements
width: 500px
---
Biquadratische Formfunktionen für 2D-Rechteck-Elemente (Q2).
```

In der Praxis der Finite-Elemente-Methode beschränkt man sich jedoch nicht ausschließlich auf Q9-Elemente, die die vollständige Polynomordnung 2 aufweisen, wie in Abbildung {numref}`pascalQuad` dargestellt. Serendipity-Elemente sind im oben beschriebenen Sinne nicht vollständig, erfüllen jedoch die Anforderungen an die räumliche Isotropie, die Verzerrungsfreiheit bei Starrkörperbewegungen und die Abbildung konstanter Verzerrungszustände. Der Begriff „Serendipity“ geht vermutlich auf ein Märchen von H. Walpole, „Die drei Prinzen von Serendip“, zurück, in dem die Prinzen die Fähigkeit besitzen, durch Zufall unverhoffte und glückliche Entdeckungen zu machen. Das zugehörige Element ist in Abbildung {numref}`quadElements` als **Q2S** dargestellt. Die Ansatzfunktionen lauten:

```{math}	
:label: 2D_Formfunktionen_Q8
\begin{align}
N_1(\xi ,\eta ) & =\frac{1}{4} (1-\eta ) (1-\xi ) (-\eta -\xi -1) & N_2(\xi ,\eta ) & =\frac{1}{4} (1-\eta ) (\xi +1) (-\eta +\xi -1) \\
N_3(\xi ,\eta ) & =\frac{1}{4} (\eta +1) (\xi +1) (\eta +\xi -1) & N_4(\xi ,\eta ) & =\frac{1}{4} (\eta +1) (1-\xi ) (\eta -\xi -1) \\
N_5(\xi ,\eta ) & =\frac{1}{2} (1-\eta ) \left(1-\xi ^2\right) & N_6(\xi ,\eta ) & =\frac{1}{2} \left(1-\eta ^2\right) (\xi +1)\\
N_7(\xi ,\eta ) & =\frac{1}{2} (\eta +1) \left(1-\xi ^2\right) &
N_8(\xi ,\eta ) & =\frac{1}{2} \left(1-\eta ^2\right) (1-\xi )
\end{align}
```

```{figure} images/shape_functions_Q8.png
---
name: quadElementsQ8N
alt: QuadElements
width: 500px
---
Biquadratische Serendipity-Formfunktionen für 2D-Rechteck-Elemente (Q2S).
```

Eine weiter Möglichkeit der zweidimensionalen Vernetzung ist die Verwendung von Dreiecken. In den gebräulichen FEM-Programmen werden dabei Dreiecke mit 3 oder 6 Knoten implementiert. Diese sind in {numref}`triangleElements` dargestellt.

```{figure} images/TriangleElements.png
---
name: triangleElements
alt: triangleElements
width: 500px
---
Dreieck-Elemente mit linearen (T1) und quadratischen (T2) Formfunktionen.
```

Die linearen Dreieck-Elemente (T1) sind die einfachsten Elemente und werden in der Literatur häufig als "konstant" bezeichnet. Dies bezieht sich auf die abgeleiteten Größen (z.B. Spannungen, Dehnungen, etc.). Diese werden als konstant dargestellt. Die Formfunktionen lauten:

```{math}	
:label: 2D_Formfunktionen_T1
\begin{align}
\lambda & = 1 - \xi - \eta & N_1(\xi ,\eta ) & = \lambda \\
N_2(\xi ,\eta ) & = \xi & N_3(\xi ,\eta ) & = \eta
\end{align}
```

```{figure} images/shape_T1.png
---
name: shapefunTria
alt: ShapeFunTria
width: 500px
---
Formfunktionen für das Dreieck-Element T1.
```


Die Formfunktionen für Dreieck-Elemente mit 6 Knoten (T2) sind vollständig quadratisch. Insgesamt hat das T2-Element bessere Approximationseigenschaften als das T1-Element. Die Formfunktionen lauten:

```{math}	
:label: 2D_Formfunktionen_T2
\begin{align}
\lambda & = 1 - \xi - \eta & N_1(\xi ,\eta ) & = \lambda (2\lambda - 1) \\
N_2(\xi ,\eta ) & = \xi (2\xi - 1) & N_3(\xi ,\eta ) & = \eta (2\eta - 1) \\
N_4(\xi ,\eta ) & = 4\lambda \xi & N_5(\xi ,\eta ) & = 4\xi \eta \\
N_6(\xi ,\eta ) & = 4\lambda \eta
\end{align}
```



### 3D Formfunktionen

```{figure} images/HexElements.png
---
name: hexElements
alt: HexElements
width: 500px
---
3D Hexaeder Elemente mit linearer (8 Knoten) und quadratischer (27 Knoten) Approximation. Zusätzlich ist das 20-Knoten Hexaeder Serendipity-Element gezeigt.
```

Im 3D-Fall werden die Formfunktionen für Hexaeder-Elemente als Tensorprodukt der 1D-Formfunktionen definiert. Für ein 3D-Hexaeder-Element mit 8 Knoten (lineares Element) sind die Formfunktionen:

```{math}	
:label: 3D_ShapeFunctions_H8
\begin{align}
N_1 &= \frac{1}{8} (1 - \xi)(1 - \eta)(1 - \zeta) & N_2 &= \frac{1}{8} (1 + \xi)(1 - \eta)(1 - \zeta) \\
N_3 &= \frac{1}{8} (1 + \xi)(1 + \eta)(1 - \zeta) & N_4 &= \frac{1}{8} (1 - \xi)(1 + \eta)(1 - \zeta) \\
N_5 &= \frac{1}{8} (1 - \xi)(1 - \eta)(1 + \zeta) & N_6 &= \frac{1}{8} (1 + \xi)(1 - \eta)(1 + \zeta) \\
N_7 &= \frac{1}{8} (1 + \xi)(1 + \eta)(1 + \zeta) & N_8 &= \frac{1}{8} (1 - \xi)(1 + \eta)(1 + \zeta)
\end{align}
```



Um ein vollständiges Element der Ordnung 2 zu erhalten, benötigt man 27 Knoten beim Hexaeder-Element. Die Formfunktionen kann man zum Beispiel {cite}`wriggers2008nonlinear` entnehmen. Mehr Verwendung findet das 20-Knoten-Element. Hierbei handelt es sich um ein Serendipity-Element, das nicht vollständig von der Ordnung 2 ist, aber die Bedingungen der Raumisotropie, keine Verformung bei starren Körperbewegungen und die Möglichkeit, konstante Spannungszustände zu modellieren, erfüllt.

Für tetraedrische Elemente implementieren gängige FEM-Programme Tetraeder mit entweder 4 oder 10 Knoten. Diese sind in Abbildung {numref}`tetraElements` dargestellt.

```{figure} images/TetElements.png
---
name: tetraElements
alt: TetraElements
width: 500px
---
Tetraedrische Elemente mit linearen (Tet1) und quadratischen (Tet2) Formfunktionen.
```

Die linearen Tetraederelemente (T4) sind die einfachsten und werden häufig als "konstante" Elemente in der Literatur bezeichnet, wobei sich auf die abgeleiteten Größen (z.B. Spannungen, Dehnungen, etc.) bezieht. Die Formfunktionen lauten:

```{math}	
:label: 3D_ShapeFunctions_T4
\begin{align}
\lambda &= 1 - \xi - \eta - \zeta & N_1(\xi ,\eta ,\zeta ) &= \lambda \\
N_2(\xi ,\eta ,\zeta ) &= \xi & N_3(\xi ,\eta ,\zeta ) &= \eta & N_4(\xi ,\eta ,\zeta ) &= \zeta
\end{align}
```

Die Formfunktionen für tetraedrische Elemente mit 10 Knoten (T10) sind vollständig quadratisch und bieten bessere Approximationseigenschaften als das T4-Element. Die Formfunktionen lauten:

```{math}	
:label: 3D_ShapeFunctions_T10
\begin{align}
\lambda &= 1 - \xi - \eta - \zeta & N_1(\xi ,\eta ,\zeta ) &= \lambda (2\lambda - 1) \\
N_2(\xi ,\eta ,\zeta ) &= \xi (2\xi - 1) & N_3(\xi ,\eta ,\zeta ) &= \eta (2\eta - 1) \\
N_4(\xi ,\eta ,\zeta ) &= \zeta (2\zeta - 1) & N_5(\xi ,\eta ,\zeta ) &= 4\lambda \xi \\
N_6(\xi ,\eta ,\zeta ) &= 4\xi \eta & N_7(\xi ,\eta ,\zeta ) &= 4\lambda \eta \\
N_8(\xi ,\eta ,\zeta ) &= 4\lambda \zeta & N_9(\xi ,\eta ,\zeta ) &= 4\xi \zeta \\
N_{10}(\xi ,\eta ,\zeta ) &= 4\eta \zeta
\end{align}
```


```{admonition} Fragen zum Kapitel
:class: warning


**Ansatzfunktionen**

- Wieviele Knoten hat ein Tetraeder-Element mit linearen Ansatzfunktionen?
- Wieviele Knoten hat ein Viereck-Element mit quadratischen Ansatzfunktionen?
- Eine wichtige Eigenschaft der gezeigten Formfunktionen ist, dass sie nur am zugehörigen Knoten den Wert 1 annehmen. An allen anderen Knoten ist der Wert 0. Was bedeuted dies für den Knotenfreiheitsgrad bzgl. seiner physikalischen Deutbarkeit?
- Welche Aussage ist richtig?
  - [ ] Bei Formfunktionen mit quadratischen Polynomgrad ist der Verlauf der Dehnungen im Element quadratisch.
  - [ ] Bei Formfunktionen mit linearen Verlauf sind die Verschiebungen im Element linear.
  - [ ] Die Dehnungen sind über die Elementränder hinweg stetig für Formfunktionen mit quadratischen Polynomgrad.
  - [ ] Die Summe der Ansatzfunktionen im Element ist stets 1.
```