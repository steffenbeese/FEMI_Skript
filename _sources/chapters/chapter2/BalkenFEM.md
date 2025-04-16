---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.7
---

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

<!-- $
\newcommand{\MtdFull}[1]{\frac{D \, #1}{D \, t}}
\newcommand{\Pd}[2]{\frac{\partial #1}{\partial #2}}
\newcommand{\d}{\text{d} }
\newcommand{\Dd}[2]{\frac{\d #1}{\d #2}}
\newcommand{\dt}[1]{#1 ^{\bullet}}
\newcommand{\dV}{\; \text{d}V}
\newcommand{\dA}{\; \text{d}A}
\newcommand{\dx}{\; \text{ d}x}
\newcommand{\dy}{\; \text{ d}y}
\newcommand{\dz}{\; \text{ d}z}
\newcommand{\dxi}{\; \text{ d}\xi}
\newcommand{\deta}{\; \text{ d}\eta}
\newcommand{\dzeta}{\; \text{ d}\zeta}
\newcommand{\grad}[1]{\text{grad}\left(#1\right)}
\renewcommand{\div}[1]{\text{div} \left(#1\right)}
\newcommand{\td}[1]{\dot{#1}}
\newcommand{\tdd}[1]{\ddot{#1}}
\newcommand{\T}{\rp{T}}
\newcommand{\rp}[1]{^{\text{#1}}}
\newcommand{\rs}[1]{_{\text{#1}}}
\renewcommand{\bm}[1]{\boldsymbol{#1}}
\newcommand{\e}{\epsilon}
\newcommand{\eb}{\bm{\e}}
\newcommand{\s}{\sigma}
\newcommand{\sb}{\bm{\sigma}}
$ -->
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

# Die Balken-FEM

```{figure} images/Balken_TM2.png
---
height: 400px
name: balken02
---
Schnittgrößen und Kinematik am Balkenelement nach {cite}`gross2007technische`
```

Als weiteren Vertreter der Strukturelemente innerhalb der Finiten Elemente Methode (FEM) betrachten wir im Folgenden die Balken-FEM. Die Impulsbilanz des Euler-Bernoulli-Balkens ist eine Differentialgleichung 4. Ordnung. Bei Voraussetzung einer konstanten Biegesteifigkeit $EI_y$ lautet diese:

```{math}
:label: balkendglsimple_2
  w^{IV} =  \frac{q(x)}{E I_y} \; .
```
Die obige **starke Form** der Balken-DGL wird im Folgenden in eine **schwache Form** umgewandelt. Formal wird die Differentialgleichung wieder mit einer beliebigen Testfunktion $\delta w$ multipliziert und über den Balken integriert. Die schwache Form lautet dann:

```{math}
:label: weakBalken_01
  EI_y \int_L \delta w'' w'' \dx - \int_L q(x) \delta w \dx - F_I \delta w_I - M_J \delta w'_J = 0 \; . 
```

Hierbei stellen $F_I$ und $M_I$ die an den Knoten angreifenden Einzellasten und Momente dar. Es ist zu beachten, dass die zweite Ableitung der Durchbiegung die höchste Ableitungsordnung in dieser Variationsgleichung darstellt. Für die Finite-Elemente-Approximation müssen ($C^1$)-stetige Ansatzfunktionen formuliert werden. Dies impliziert, dass an den Knotenpunkten die Verschiebungsansätze sowohl in der Durchbiegung ($w$) als auch in der Neigung ($w'$) stetig sein müssen. Anders als beim Stabelement sind die Freiheitsgrade an den Knotenpunkten nicht nur die Verschiebungen, sondern auch die Neigungen. Dies ist charakteristisch für die Formulierung von Balkenelementen, aber auch für die Formulierung von Plattenelementen und Schalenelementen.


## Ansatzfunktionen

```{figure} images/Balkenelement.png
---
height: 300px
name: balken03
---
Balkenelement mit lokalen Freiheitsgraden
```

Analog zum Stabelement führen wir die normierte Koordinate $\xi=\frac{x}{\ell_e}$ ein. Die Ansatzfunktionen für die Durchbiegung $w_h$ wird dann als Linearkombination der Formfunktionen $N_I$ und der Freiheitsgrade $w_I$ geschrieben: 

```{math}
:label: Ansatz_Balken
\begin{align}
  w_h & = N_1(\xi) w_1 + N_2(\xi) \psi_1 \ell_e +  N_3(\xi) w_2 + N_4(\xi) \psi_2 \ell_e = \sum_{I=1}^{4} N_I(\xi) w_I \\
  & = \begin{pmatrix} N_1(\xi) & N_2(\xi)\ell_e & N_3(\xi) & N_4(\xi)\ell_e \end{pmatrix} \begin{pmatrix} w_1 \\ \psi_1 \\ w_2 \\ \psi_2 \end{pmatrix}
\end{align}
```

Der Vektor der Freiheitsgrade $\bm{w}=w_I$ enthält die Durchbiegung $w$ und die N $\psi$ an den beiden Knoten des Balkenelements. Wie bei den Formfunktionen des Stabelementes, ist die Formfunktion $N_I$ nur für den Freiheitsgrad $w_I$ identisch mit 1 und für alle anderen Freiheitsgrade gleich 0. Formfunktionen die diese Eigenschaft haben, sind zum Beispiel die Hermite-Polynome:
```{math}
:label: Formfunktion_Balken
\begin{align}
  \bm{N} = \begin{pmatrix} 
  1-3\xi^2+2\xi^3 \\ 
  (\xi-2\xi^2+\xi^3)\ell_e \\ 
  3\xi^2-2\xi^3 \\ 
  (-\xi^2+\xi^3)\ell_e \end{pmatrix}
\end{align}
```


<!-- 
```{code-cell} ipython3
---
mystnb:
  figure:
    caption: Formfunktionen für ein Balkenelelement mit Hermite-Interpolation.
    name: Shape-Beam
tags: [
  "remove-input"
  ]
---
import numpy as np
import plotly.graph_objects as go

# Define the x values
x = np.linspace(0, 1, 100)

# Define the shape functions - Hermite polynomials 4th order
N1 = 1 - 3*x**2 + 2*x**3
N2 = x - 2*x**2 + x**3
N3 = 3*x**2 - 2*x**3
N4 = -x**2 + x**3

# Create the figure
fig = go.Figure()

# Add traces for each shape function
fig.add_trace(go.Scatter(x=x, y=N1, mode='lines', name='N_1'))
fig.add_trace(go.Scatter(x=x, y=N2, mode='lines', name='N_2'))
fig.add_trace(go.Scatter(x=x, y=N3, mode='lines', name='N_3'))
fig.add_trace(go.Scatter(x=x, y=N4, mode='lines', name='N_4'))

# Update layout with LaTeX math
fig.update_layout(
    xaxis_title=r'$\eta$',
    yaxis_title=r'$N(\eta)$',
    legend=dict(x=1.05, y=1),
    font=dict(family="Serif", size=15),
    template='plotly_white',
    xaxis=dict(showgrid=True, gridcolor='grey', gridwidth=0.6, griddash='dash'),
    yaxis=dict(showgrid=True, gridcolor='grey', gridwidth=0.6, griddash='dash')
)

# Show the plot
# fig.show()
``` -->

```{code-cell} ipython3
---
mystnb:
  figure:
    caption: Formfunktionen für ein Balkenelelement mit Hermite-Interpolation.
    name: Shape-Beam
tags: [
  "remove-input"
  ]
---

import numpy as np
import matplotlib.pyplot as plt
import scienceplots

# Set up the plot configurations.
plt.rcParams['lines.linewidth'] = 2.0;
plt.rcParams['lines.color'] = 'black';
plt.rcParams['legend.frameon'] = True;
plt.rcParams['figure.figsize'] = (8, 6);
plt.rcParams['font.family'] = 'serif';
plt.rcParams['legend.fontsize'] = 15;
plt.rcParams['font.size'] = 15;
plt.rcParams['axes.spines.right'] = False;
plt.rcParams['axes.spines.top'] = False;
plt.rcParams['axes.spines.left'] = True;
plt.rcParams['axes.spines.bottom'] = True;
plt.rcParams['axes.axisbelow'] = True;
plt.rcParams['grid.color'] = 'grey';
plt.rcParams['grid.alpha'] = 0.6;
plt.rcParams['grid.linestyle'] = '--';

plt.style.use(['science', 'grid']);


fig,ax = plt.subplots(1,1);

# Define the x values
x = np.linspace(0, 1, 100);
# Define the shape functions - Hermite polynomials 4th order
N1 = 1 - 3*x**2 + 2*x**3;
N2 = x - 2*x**2 + x**3;
N3 = 3*x**2 - 2*x**3;
N4 = -x**2 + x**3;
# Plot the shape functions
ax.plot(x, N1, label='$N_1$');
ax.plot(x, N2, label='$N_2$');
ax.plot(x, N3, label='$N_3$');
ax.plot(x, N4, label='$N_4$');
# Add labels and legend
ax.set_xlabel(r'$\eta$');
ax.set_ylabel(r'$N(\eta)$');
ax.legend(bbox_to_anchor=(1.05, 1), loc='upper left'); 

```

```{admonition} Wichtige Eigenschaften des Ansatzes des Balkens
:class: important
- Die Durchbiegung $w$ ist $C^1$-stetig
- Die Durchbiegung $w$ und die Neigung $\psi=w'$ sind über die Elementgrenzen hinweg stetig 
- Durch die Wahl der Formfunktionen haben die Knotenfreiheitsgrade die Bedeutung von Durchbiegung und Neigung
```

Auch für die Testfunktion $\delta w$ wird der Ansatz {eq}`Ansatz_Balken` verwendet. 

## Elementsteifigkeitsmatrix (Diskretisierung Term 1 in Gleichung {eq}`weakBalken_01` )

Eingesetzt in die Schwache Form liefert der erste Term die Elementsteifigkeitsmatrix. Hierfür müssen zunächst die Ableitungen des Ansatzes bzgl. $x$ berechnet werden. Aus 
 $\dx = \ell_e \dxi$ folgt:
```{math}
:label: Differentialquotient
\begin{align}
  \Dd{w}{x} & = \frac{1}{\ell_e} \Dd{w}{\xi} = \frac{1}{\ell_e} \bm{N}_{,\xi} \bm{w} \\
  \Dd{^2 w}{x^2} & = \frac{1}{\ell_e^2} \Dd{^2 w}{\xi^2} = \frac{1}{\ell_e^2} \bm{N}_{,\xi\xi} \bm{w}
\end{align}
```

Eingesetzt in die Schwache Form ergibt sich für die Elementsteifigkeit:


```{math}
:label: Elementsteifigkeit_Balken_01
\begin{align}
  & \delta \bm{w}^T \frac{EI}{\ell^3_e} \int_0^{1}  \bm{N}_{,\xi\xi}^T \bm{N}_{,\xi\xi} \dxi\, \bm{w} \\
  &= \delta \bm{w}^T \frac{EI}{\ell^3_e} \int_0^{1} \begin{pmatrix}
    12\xi -6 & (6\xi-4)\ell_e  & -12\xi+6 & (6\xi-2)\ell_e 
  \end{pmatrix} \begin{pmatrix}
    12\xi -6 \\
    (6\xi-4)\ell_e  \\
    -12\xi+6 \\
    (6\xi-2)\ell_e 
  \end{pmatrix} \dxi\, \bm{w} \\
  &= \delta \bm{w}^T \frac{EI}{\ell^3_e} \int_0^{1} \begin{pmatrix}\left(12 \xi - 6\right)^{2} & \ell_e \left(6 \xi - 4\right) \left(12 \xi - 6\right) & \left(6 - 12 \xi\right) \left(12 \xi - 6\right) & \ell_e \left(6 \xi - 2\right) \left(12 \xi - 6\right)\\\ell_e \left(6 \xi - 4\right) \left(12 \xi - 6\right) & \ell_e^{2} \left(6 \xi - 4\right)^{2} & \ell_e \left(6 - 12 \xi\right) \left(6 \xi - 4\right) & \ell_e^{2} \left(6 \xi - 4\right) \left(6 \xi - 2\right)\\\left(6 - 12 \xi\right) \left(12 \xi - 6\right) & \ell_e \left(6 - 12 \xi\right) \left(6 \xi - 4\right) & \left(6 - 12 \xi\right)^{2} & \ell_e \left(6 - 12 \xi\right) \left(6 \xi - 2\right)\\\ell_e \left(6 \xi - 2\right) \left(12 \xi - 6\right) & \ell_e^{2} \left(6 \xi - 4\right) \left(6 \xi - 2\right) & \ell_e \left(6 - 12 \xi\right) \left(6 \xi - 2\right) & \ell_e^{2} \left(6 \xi - 2\right)^{2}\end{pmatrix} \dxi\, \bm{w} \\
  &= \delta \bm{w} \underbrace{\frac{EI}{\ell^3} \begin{pmatrix}12 & 6 \ell_e & -12 & 6 \ell_e\\6 \ell_e & 4 \ell_e^{2} & - 6 \ell_e & 2 \ell_e^{2}\\-12 & - 6 \ell_e & 12 & - 6 \ell_e\\6 \ell_e & 2 \ell_e^{2} & - 6 \ell_e & 4 \ell_e^{2}\end{pmatrix}}_{\bm{K_e}} \,  \bm{w}
\end{align}
```

## Diskretisierung der Streckenlast 

Die Streckenlast $q(x)$ hat im Element $e$ im Allgemeinen einen beliebigen Verlauf. Um die Streckenlast zu diskretisieren wird angenommen, dass die Streckenlast im Element linear veränderlich ist. Es wird also der folgende Ansatz gemacht:

```{math}
:label: AnsatzStreckenlast

\begin{align}
  q_h(\xi) & = (1-\xi) q_1 + \xi q_2 \\
  & = \sum_I N_I(\xi)  q_I
\end{align}
```

Wird dieser Ansatz in die schwache Form eingesetzt, so ergibt sich:


```{math}
:label: weak_form_streckenlast_01
\begin{align}
  & \int_L \delta \bm{w}  q(x) \dx \\
  & = \ell \int_{0}^1  \delta \bm{w}(\xi) q_h(\xi)  \dxi \\
  & \delta \bm{w} \ell \int_{0}^1 \begin{pmatrix} 2 \xi^{3} - 3 \xi^{2} + 1\\\ell \left(\xi^{3} - 2 \xi^{2} + \xi\right)\\- 2 \xi^{3} + 3 \xi^{2}\\\ell \left(\xi^{3} - \xi^{2}\right)\end{pmatrix} \begin{pmatrix} q_1 & q_2  \end{pmatrix} \dxi \\
  & = \delta \bm{w} \ell \int_{0}^1 \begin{pmatrix}
  \left(q_{1} \left(1 - \xi\right) + q_{2} \xi\right) \left(2 \xi^{3} - 3 \xi^{2} + 1\right)\\\ell \left(q_{1} \left(1 - \xi\right) + q_{2} \xi\right) \left(\xi^{3} - 2 \xi^{2} + \xi\right)\\\left(- 2 \xi^{3} + 3 \xi^{2}\right) \left(q_{1} \left(1 - \xi\right) + q_{2} \xi\right)\\\ell \left(\xi^{3} - \xi^{2}\right) \left(q_{1} \left(1 - \xi\right) + q_{2} \xi\right)
  \end{pmatrix} \\
  & = \delta \bm{w} \underbrace{ \ell \begin{pmatrix}
  \frac{7 q_{1}}{20} + \frac{3 q_{2}}{20}\\\frac{\ell q_{1}}{20} + \frac{\ell q_{2}}{30}\\\frac{3 q_{1}}{20} + \frac{7 q_{2}}{20}\\- \frac{\ell q_{1}}{30} - \frac{\ell q_{2}}{20}
  \end{pmatrix}}_{\bm{F}_Q}
\end{align}
```

## Beispiel der Balken FEM

```{figure} images/Balken_Problem.png
---
height: 81px
name: balken03
---
Statisch bestimmt gelagerter Balken mit konstanter Streckenlast
```
Für das dargestellte Beispiel eines statisch bestimmten Balkens mit konstanter Streckenlast lässt sich die Lösung der Verschiebung durch vierfache Integration der Differentialgleichung {eq}`balkendglsimple_2` bestimmen. Die Lösung lautet:

```{math}
:label: Balken_example_01
EI w(x) =\frac{\ell^{3} q_{0} x}{24} - \frac{\ell q_{0} x^{3}}{12} + \frac{q_{0} x^{4}}{24}
```

Über die Beziehungen:

```{math}
:label: Balken_example_02
\begin{align}
EI w'' & =-M(x) =  \frac{q_{0} x \left(\ell - x\right)}{2} \\
EI w'''' &= -Q(x) = q_{0} \left(\frac{\ell}{2} - x\right)
\end{align}
```
erhält man die Schnittgrößen für das Biegemoment und die Querkraft. 

```{code-cell} ipython3
---
mystnb:
  figure:
    caption: Verlauf der Durchbiegung und des 
    name: Schnittgrößen_Balken
tags: [
  "hide-input"
  ]
---
import sympy as sp
from sympy import Rational
from sympy import Eq
from sympy.plotting import plot, PlotGrid


x,EI,ell,q0,C_1,C_2,C_3,C_4 = sp.symbols('x,EI,ell,q0,C_1,C_2,C_3,C_4')

# Biegelinie
w = 1/EI*(Rational(1,24)*q0*x**4 +Rational(1,6)*C_1*x**3+Rational(1,2)*C_2*x**2+C_3*x+C_4)

# Bestimmung der Integrationskonstanten
eq1 = Eq(w.subs(x,0),0)
eq2 = Eq(w.subs(x,ell),0)
eq3 = Eq(-w.diff(x,2).subs(x,0),0)
eq4 = Eq(-w.diff(x,2).subs(x,ell),0)
solution = sp.solve((eq1,eq2,eq3,eq4),(C_1,C_2,C_3,C_4))

# Plot der Biegelinie und Schnittgrößen
wp = w.subs(solution).subs({"ell":1,"q0":1,"EI":1})
M = -wp.diff(x,2)
Q = -wp.diff(x,3)

p1= plot(wp,(x,0,1),title=r'Durchbiegung $\frac{EI}{q_0} w$',show=False,xlabel=r'$\frac{x}{\ell}$',ylabel=r'$\frac{EI}{q_0} w$',line_color='black')
p2= plot(M,(x,0,1),title=r'Biegemoment $\frac{1}{q_0} M$',show=False,xlabel=r'$\frac{x}{\ell}$',ylabel=r'$\frac{1}{q_0}M$',line_color='red')
p3= plot(Q,(x,0,1),title=r'Querkraft $\frac{1}{q_0}Q$',show=False,xlabel=r'$\frac{x}{\ell}$',ylabel=r'$\frac{1}{q_0}Q$',line_color='blue')


PlotGrid(1,3,p1,p2,p3,size=(15,5));

```


Wir lösen nun dieses Problem mit der obigen Balken-FEM und vergleichen die Ergebnisse für die Durchbiegung und die Schnittgrößen mit der analytischen Lösung.


```{figure} images/Flächenträgheitsmomente.png
---
height: 600px
name: FT
---
Flächenträgheitsmomente für I-Profile nach DIN 1025-1:2009-04. [ezzart.org](https://www.ezzat.org/de/Querschnittswerte/gewalzt/I/i.php)

```


## Konvergenz 