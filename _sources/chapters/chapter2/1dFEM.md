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

# Die Schwache Form am Beispiel des Stabes

```{figure} images/Stab.png
---
height: 400px
name: stab_fig
---
Stab unter Eigengewicht und externer Kraft.
```
Auch hier beginnen wir mit der starken Form der Differentialgleichung:
```{math}
:label: stabdglsimple_2
 {EA} u^{\prime \prime} =  - {n(x)} A \; ,
```

```{admonition} Analytische Lösung
:class: tip
Die analytische Lösung erhalten wir durch zweifache Integration und Einsetzen der Randbedingungen:
  \begin{equation}
  u(x) = \left( \frac{F}{EA} + \frac{n \ell}{E}\right)\cdot x - \frac{1}{2} \frac{n}{E} x^2 \; . 
  \end{equation}
Die Schnittkraft $N$ berechnet sich wie folgt:
  \begin{equation}
  N(x) = F + n A (\ell-x). 
  \end{equation}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Zur Herleitung der Finite-Elemente-Form multiplizieren wir die starke Form mit der Testfunktion $\delta u$ und integrieren die Gleichung über das Gebiet:
```{math}
:label: stabdglsimple_weak
 \int_{0}^{\ell} \delta u u^{\prime \prime} \dx = \int_{0}^{\ell} - \delta u \frac{n(x)}{EA} \dx\; .
```
Der Ausdruck auf der linken Seite wird umgeformt zu:
```{math}
:label: stabdglsimple_product
 \int_{0}^{\ell} \delta u u^{\prime \prime} \dx = \int_{0}^{\ell} \left(\delta u u^{\prime}\right)^{\prime} - \delta u^{\prime}  u^{\prime}  \dx\; .
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Ersetzen wir nun $\delta u^{\prime}=\delta \e$ und $u^{\prime}=\e$ und setzen die obige Produktregel in Gleichung {eq}`stabdglsimple_weak`, so erhalten wir:
```{math}
:label: stabdglsimple_weak2
\begin{align}
 \int_{0}^{\ell} \delta \e \cdot E\e \, A \dx & = \left[\delta u \cdot \sigma A \right]_0^{\ell} + \int_0^{\ell} \delta u \cdot n(x) \dx\;  \\
 & = \left[ \delta u(\ell) \cdot F + \delta u(0) \cdot 0\right]+ \int_0^{\ell} \delta u \cdot n(x) \dx\; .
 \end{align}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Das Verfahren von Ritz

Als Einführung in die Finite-Element-Methoden beginnen wir mit dem Verfahren, welches von Walter Ritz (1878 - 1909) eingeführt wurde. Dies fußt auf dem Prinzip der virtuellen Arbeit, formuliert als $\delta U − \delta W = 0$, wobei $\delta U$ für die Arbeit der inneren Kräfte und $\delta W$ für die der äußeren Kräfte steht. Für einen einfachen Stab ergibt sich die obige schwache Form {eq}`stabdglsimple_weak2`.

Der nächste Schritt ist die Aufstellung einer Näherungslösung $u_h$ für das Verschiebungsfeld $u$, zum Beispiel durch eine quadratische Funktion:

```{math}
u_h(x) = a_0 + a_1 x + a_2 x^2,
```

wobei die Koeffizienten $a_i$ durch Minimierung der virtuellen Arbeit ermittelt werden müssen. Diese Parameter müssen gleichzeitig kinematische Verträglichkeit garantieren und somit die geometrischen Randbedingungen einhalten. Aus $u(0) = 0$ ergibt sich dann $a_0 = 0$. Die benötigte Ableitung des Näherungsansatzes lautet daher:

```{math}
\e = u^{\prime} = a_1 + 2a_2 x.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Die Variationen dieses Ansatzes werden durch Variieren der Koeffizienten gebildet:

```{math}
\delta u(x) = \delta a_1 x + \delta a_2 x^2
```

und

```{math}
\delta \e= \delta a_1 + 2 \delta a_2 x.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Eingesetzt in das Prinzip der virtuellen Arbeit führt es zu:

```{math}
EA \int_0^{\ell} (\delta a_1 + 2 \delta a_2 x) (a_1 +2 a_2 x) \dx = F (a_1 \ell +a_2 \ell^2) + \int_0^{\ell} (\delta a_1 x + \delta a_2 x^2) n A \dx \; .
```

Nach Integration und Herausziehen der Variationen resultiert daraus:

```{math}
\delta a_1\left[EA \ell (a_1 + a_2 \ell) - \frac{1}{2} n A \ell^2 - F \ell\right] + \delta a_2 \left[EA\ell^2(a_1 + \frac{4}{3}a_2\ell) - \frac{1}{3}nA\ell^3 - F\ell^2\right] = 0.
```

Da die Variationen beliebig sind, müssen die in eckigen Klammern stehenden Terme jeweils null sein. Daraus folgt ein lineares Gleichungssystem zur Bestimmung der Koeffizienten, das gelöst wird als:

```{math}
a_1 = \frac{F}{EA} + \frac{n \ell}{E} \qquad \text{und} \qquad  a_2 = -\frac{n}{2E}.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Somit erhalten wir als Näherungslösung für diesen Fall:

```{math}
u_h(x) = \left(\frac{F}{EA} + \frac{n \ell}{E}\right)x - \left(\frac{n}{2E}\right)x^2.
```

Man kann leicht erkennen, dass mit diesem quadratischen Ansatz die analytische Lösung der Differentialgleichung rekonstruiert werden konnte.


+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


```{admonition} Problem
:class: warning
Ein Problem beim Ritz'schen Verfahren ist die Wahl der Ansatzfunktion. Diese muss die Randbedingungen erfüllen und die Stetigkeitsanforderungen der schwachen Form. Für komplexere Probleme ist die Wahl der Ansatzfunktion nicht trivial und oft auch nicht möglich.
```

## Finite Elemente Formulierung für den Stab

### Diskretisierung der schwachen Form

```{figure} images/Stab_element.png
---
width: 600px
name: stabelement
---
Stab unter Eigengewicht und externer Kraft mit Stabelementen diskretisiert.
```
Im vorangegangenen Abschnitt haben wir die Effektivität des Prinzips der virtuellen Arbeit in Kombination mit dem Ritz'schen Verfahren untersucht. Es erweist sich jedoch oft als schwierig, einen angemessenen Näherungsansatz für komplexe Strukturen zu finden, die einer vielschichtigen Belastung ausgesetzt sind. Die Finite-Elemente-Methode (FEM) löst dieses Problem, indem sie einfache Ansatzfunktionen auf überschaubaren Teilbereichen – den sogenannten finiten Elementen – anwendet und diese dann systematisch für das gesamte Gebiet integriert.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Als Beispiel hierfür dient die im Bild {numref}`stabelement` dargestellte Diskretisierung eines Stabes mit der Gesamtlänge $\ell$, der in zwei finite Elemente mit jeweils der Länge $l_i = \frac{\ell}{2}$ unterteilt wird. 

```{admonition} Allgemeines zu Finiten Elementen
:class: tip

Ein Finites Element besteht in dem vorliegenden Beispiel aus 2 Knoten. Sowohl die Knoten, als auch die Elemente werden im Allgemeinen nummeriert, damit man sie eindeutig ansprechen kann. Im Bild sind die Elementnummern in den rechteckigen Kästen neben dem Element eingetragen. Die Knotennummer sind in den Kreisen neben den Knoten dargestellt. Die Knoten sind die Träger der primären Feldvariablen (hier: Verschiebung $u$) und Ziel der Finiten Elemente Berechnung ist die Bestimmung der primären Variablen, auch Freiheitsgrade (English: Degree of freedom **Dof**) an den Knoten. Im Bild {numref}`stabelement` auf der rechten Seite ist ein einzelnes Stabelement dargestellt. Das Stabelement hat 2 lokale Knoten und um die Verschiebung an diesen Knoten eindeutig zuzuordnen verwenden wir die Notation:

$
u_1^{e} \text{ Verschiebung u am lokalen Knoten 1 von Element e}
$

$
u_2^{e} \text{ Verschiebung u am lokalen Knoten 2 von Element e}
$
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Auf diesen Elementen werden dann einfache Ansatzfunktionen verwendet, welche eine lineare Approximation des Verschiebungsfeldes darstellen:

```{math}
:label: LinearDisplacementAnsatz
\begin{align}
 N_1(\xi) & = (1-\xi) \qquad N_2(\xi)= \xi  \\
 u_h(\xi) & = N_1(\xi) u_1^{(e)}+ N_2(\xi) u_2^{(e)} \\
 u_h(\xi) & = \sum_{I=1}^2 N_I(\xi) u_I^{(e)}
 \end{align}
```

hierbei wurde das lokale Koordinatensystem $\xi = \frac{x}{\ell_e} \; \in \; [0,1]$  eingeführt, damit der gewählte Ansatz für alle Stabelmenete, unabhängig der Stablänge $\ell_e$ gültig ist. In der Matrixschreibweise erhält man für den Ausdruck {eq}`LinearDisplacementAnsatz`:

```{math}
:label: LinearDisplacementAnsatz2
\begin{align}
  u_h(\xi) & = \underbrace{\begin{bmatrix} (1-\xi) & \xi \end{bmatrix}}_{\bm{N}\T} \underbrace{\begin{bmatrix} u_1^{(e)} \\ u_2^{(e)}\end{bmatrix}}_{\bm{u}^{(e)}} = \bm{N}\T \bm{u}^{(e)}
 \end{align}
```

```{code-cell} ipython3
:tags: ["hide-input"]

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import Circle

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

xi = np.linspace(0, 1, 100)

# Create a figure and axis.
fig, ax = plt.subplots(1, 1)

# Plotting the functions.
ax.plot(1-xi, xi)
ax.plot(xi, xi)

# Draw black line from (0,0) to (1,0)
ax.plot([0, 1], [0, 0], color='black', linewidth=5.0)

# Draw red dots at (0,0) and (1,0)
ax.plot(0, 0, 'ro', markersize=10)
ax.plot(1, 0, 'ro', markersize=10)

# Annotate the red dots.
# Draw a circle and add text at the specified location
circle = Circle((-0.1, 0.0), 0.05, color='red', fill=False,linewidth=2)
circle2 = Circle((1.1, 0.0), 0.05, color='red', fill=False,linewidth=2)
ax.add_patch(circle)
ax.add_patch(circle2)
ax.annotate('1', xy=(0, 0), xytext=(-0.11, -0.02), color='red')
ax.annotate('2', xy=(1, 0), xytext=(1.08, -0.02), color='red')

# Set labels for the axis
ax.set_xlabel(r"$\xi$")
ax.set_ylabel(r"$N(\xi)$")

# Draw background grid
ax.grid(True)

# Add legend.
ax.legend([r"$N_1(\xi)$", r"$N_2(\xi)$"])

```
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Zur Berechnung des Prinzips der virtuellen Verrückungen bzw. der schwachen Form wird zusätzlich die Ableitung des Ansatzes für $u_h$ benötigt. Diese repräsentiert die Dehnung des Stabes $\e = \Dd{u}{x}$. Da der Ansatz über die normalisierte Koordinate $\xi$ bestimmt wurde berechnet sich die Ableitung über die Kettenregel:

```{math}
\e_h = \Dd{u_h}{x} = \Dd{u_h}{\xi} \underbrace{\Dd{\xi}{x}}_{\frac{1}{\ell_e}} = \frac{1}{\ell_e} \begin{bmatrix} -1 & 1 \end{bmatrix} \begin{bmatrix} u_1^{(e)} \\ u_2^{(e)}\end{bmatrix} 
```

Für die Variation $\delta u$ und $\delta \e$ wählen wir den gleichen Ansatz:

```{math}
\delta u_h = \begin{bmatrix}  (1-\xi) & \xi \end{bmatrix} \begin{bmatrix} \delta u_1^{(e)} \\ \delta u_2^{(e)}\end{bmatrix} \qquad \delta \e_h = \frac{1}{\ell_e} \begin{bmatrix} -1 & 1 \end{bmatrix} \begin{bmatrix} \delta u_1^{(e)} \\ \delta u_2^{(e)}\end{bmatrix} 
```


Betrachten wir nun das Prinzip der virtuellen Verrückungen in Gleichung {eq}`stabdglsimple_weak2`:

```{math}
\begin{align}
 \textcolor{green}{\int_{0}^{\ell} \delta \e \cdot E\e \, A \dx} -\textcolor{red} {\int_0^{\ell} \delta u \cdot n A \dx} - \textcolor{blue}{\delta u(\ell) \cdot F} = 0\; 
 \end{align}
```

und setzen hier unseren gewählten Ansatz für $u$, $\e$, $\delta u$ und $\delta \e$ ein, dann erhalten wir die folgende Gleichung:


```{math}
:label: stabFEM1
\begin{align}
 \textcolor{green}{\int_{0}^{1} \frac{1}{\ell_e} \begin{bmatrix} -1 & 1 \end{bmatrix} \begin{bmatrix} \delta u_1^{(e)} \\ \delta u_2^{(e)}\end{bmatrix}  EA \frac{1}{\ell_e} \begin{bmatrix} -1 & 1 \end{bmatrix} \begin{bmatrix} u_1^{(e)} \\ u_2^{(e)}\end{bmatrix}   \underbrace{\ell_e \d \xi}_{\dx}} \\
 - \textcolor{red}{\int_0^{1} \begin{bmatrix} (1-\xi) & \xi \end{bmatrix} \begin{bmatrix} \delta u_1^{(e)} \\ \delta u_2^{(e)}\end{bmatrix}  n A \ell_e \d \xi} - \textcolor{blue}{\begin{bmatrix} \delta u_1^{(e)} & \delta u_2^{(e)}\end{bmatrix} \begin{bmatrix} S_1^{(e)} \\ S_2^{(e)} \end{bmatrix}}  = 0\; 
\end{align}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Die Variation der Knotenverschiebung $\delta u_I$ und die Knotenverschiebung $u_I$ sind nicht abhängig von $\xi$ und können somit aus dem Integranden herrausgezogen werden:

```{math}
:label: stabFEM2
\begin{align}
 \begin{bmatrix} \delta u_1^{(e)} & \delta u_2^{(e)}\end{bmatrix}\left\{ \textcolor{green}{   \int_{0}^{1} EA \frac{1}{\ell_e} \begin{bmatrix} 1 & -1 \\ -1 & 1\end{bmatrix}  \d \xi \begin{bmatrix} u_1^{(e)} \\ u_2^{(e)}\end{bmatrix}} \\
 - \textcolor{red}{n \ell_e A \int_0^{1} \begin{bmatrix} (1-\xi)  \\ \xi \end{bmatrix}   \d \xi} - \textcolor{blue}{ \begin{bmatrix} S_1^{(e)} \\ S_2^{(e)} \end{bmatrix}} \right\}  = 0\; 
\end{align}
```

Da die Variation beliebig ist, muss die Gleichung für jeden Faktor separat erfüllt sein. Somit erhält man für jedes Element die Gleichung:

```{math}
:label: stabFEM3
\begin{align}
  \textcolor{green}{   \underbrace{\int_{0}^{1} EA \frac{1}{\ell_e} \begin{bmatrix} 1 & -1 \\ -1 & 1\end{bmatrix}  \d \xi}_{\bm{K}^{(e)}} \begin{bmatrix} u_1^{(e)} \\ u_2^{(e)}\end{bmatrix}} 
 - \textcolor{red}{ \underbrace{n \ell_e A \int_0^{1} \begin{bmatrix} (1-\xi) \\ \xi  \end{bmatrix}   \d \xi}_{\bm{F}_V^{(e)}}} - \textcolor{blue}{ \underbrace{\begin{bmatrix} S_1^{(e)} \\ S_2^{(e)} \end{bmatrix}}_{\bm{F}_K^{(e)}}}  & = 0\; \\
 \textcolor{green}{\bm{K}^{(e)} \bm{u}^{(e)}} &=  \textcolor{red}{\bm{F}_V^{(e)}} + \textcolor{blue}{\bm{F}_K^{(e)} } 
\end{align}
```
Dabei ist $\bm{K}^{(e)}$ die **Elementsteifigkeitsmatrix**, $\bm{F}_V^{(e)}$ ist der **Vektor der äquivalenten Knotenkräfte** aus den verteilten Lasten und $\bm{F}_K^{(e)}$ sind die an den Knotenpunkten **eingeprägten Kräfte**. Für dieses einfache Element können die Integrale in Gleichung {eq}`stabFEM3` analytisch berechnet werden. Nach der Integration erhält man:


```{math}
:label: stabFEM4
\begin{align}
 \bm{K}^{(e)} & = EA \frac{1}{\ell_e} \begin{bmatrix} 1 & -1 \\ -1 & 1\end{bmatrix} \\
 \bm{F}_V^{(e)} & = n \ell_e A  \begin{bmatrix} \frac{1}{2} \\ \frac{1}{2} \end{bmatrix} 
\end{align}
```

Damit wurde die Differentialgleichung 2. Ordnung in eine algebraische Gleichung umgeformt. Wir haben jetzt für die zwei Finiten Elemente zwei **lokale**, lineare Gleichungssysteme. Diese sind jedoch nicht unabhängig voneinander und müssen im nächsten Schritt in ein **globales** Gleichungssystem assembliert werden.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


### Assemblierung der Finiten Elemente

Das Ziel dieses Abschnitts ist es, die Entwicklung der Gleichungen für das Gesamtsystem aus den Elementsteifigkeitsmatrizen zu beschreiben. Wir werden die Assemblierungoperationen vorstellen, die hierfür verwendet werden. Diese Operationen sind ein fester Bestandteil der Finite-Elemente-Methode (FEM) und kommen selbst bei den komplexesten Problemen zum Einsatz. Daher ist es wesentlich, diese Verfahren zu beherrschen, um die FEM zu erlernen.

Die Elemente im unserem dargestellten Beispiel ({numref}`stabelement`) werden mit den Nummern 1 und 2 bezeichnet, während die Knoten von 1 bis 3 nummeriert sind; weder die Knoten noch die Elemente müssen in einem FEM-Netz in einer bestimmten Reihenfolge nummeriert sein. Wir kommen hierzu nochmal später in der Vorlesung.

Beachten Sie, dass die Kraftgrößen der Elemente $S_i^{(e)}$ mit den Indizes 1 und 2 versehen sind. Dies sind die lokalen Knotennummern. Die Knoten des Netzes sind die globalen Knotennummern. Die lokalen Knotennummern eines Stabelements sind immer in der positiven $\xi$-Richtung mit 1, 2 nummeriert. Die globalen Knotennummern hingegen sind willkürlich.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Wir versuchen nun, die lokalen Kräfte $S_i^{(e)}$ für das Element 1 mit den globalen Verschiebungen in Verbindung zu bringen. Betrachten wir zunächst den Knoten 2, welchen sich Element 1 und 2 teilen. Hier gilt für die Verschiebung:
\begin{align}
  u_2^{(1)} = u_1^{(2)} = u_2 \; .
\end{align}

```{math}
:label: stabFEM5
\begin{align}
\begin{bmatrix}
0 \\
S_2^{(1)}\\
S_1^{(1)}
\end{bmatrix} + \frac{1}{2}n\ell_1 A_1 \begin{bmatrix}
0 \\
1\\
1
\end{bmatrix} = \frac{E_1A_1}{\ell_1}\begin{bmatrix}
0 & 0 & 0\\
0 & 1 & -1\\
0 & -1 & 1
\end{bmatrix} \begin{bmatrix}
u_1 \\
u_2\\
u_3
\end{bmatrix}
\end{align} \; .
```
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Dazu haben wir lediglich das Elementgleichungssystem {eq}`stabFEM3` genommen und die lokalen Freiheitsgrade $u_i^{(e)}$ durch ihre zugeordneten globalen Freiheitsgrade $u_i$ gemäß den globalen Knotennummern ersetzt. Gleichzeitig haben wir bereits eine zusätzliche Gleichung (1. Zeile) eingeführt. Deren Sinn wird gleich ersichtlich. Das gleiche Vorgehen wenden wir nun auf das Element 2 an:

```{math}
:label: stabFEM6
\begin{align}
\begin{bmatrix}
S_2^{(2)} \\
S_1^{(2)} \\
0
\end{bmatrix} + \frac{1}{2}n\ell_2 A_2 \begin{bmatrix}
0 \\
1\\
1
\end{bmatrix} = \frac{E_2A_2}{\ell_2}\begin{bmatrix}
1 & -1 & 0\\
-1 & 1 & 0\\
0 & 0 & 0
\end{bmatrix} \begin{bmatrix}
u_1 \\
u_2\\
u_3
\end{bmatrix}
\end{align} \; .
```

Nun liegen uns zwei Gleichungssysteme vor, welche sich auf die gleichen Freiheitsgrade beziehen. Durch eine Addition erhalten wir schließlich:


```{math}
:label: stabFEM7
\begin{align}
\begin{bmatrix}
S_2^{(2)} \\
S_1^{(2)} + S_2^{(1)} \\
S_1^{(1)}
\end{bmatrix} + \frac{1}{4}n\ell A \begin{bmatrix}
1 \\
1+1\\
1
\end{bmatrix} = \frac{EA}{\ell}\begin{bmatrix}
1 & -1 & 0\\
-1 & 2 & -1\\
0 & -1 & 1
\end{bmatrix} \begin{bmatrix}
u_1 \\
u_2\\
u_3
\end{bmatrix}
\end{align} \; .
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Es bleibt noch übrig die Stabkräfte (innere Kräfte) $S_i$ mit den externen Kräften in Beziehung zu setzen. Für jeden Knoten kann das Kräftegleichgewicht aufgestellt werden:

```{math}
:label: stabFEM8
\underbrace{\begin{bmatrix}
S_2^{(2)} \\
S_1^{(2)} \\
0
\end{bmatrix}}_{\bm{F}_K^{(2)}} + \underbrace{\begin{bmatrix}
0 \\
S_2^{(1)} \\
S_1^{(1)}
\end{bmatrix}}_{\bm{F}_K^{(1)}} = \underbrace{\begin{bmatrix}
F \\
0 \\
0
\end{bmatrix}}_{\bm{F}\rs{ext}} + \underbrace{\begin{bmatrix}
0 \\
0 \\
R
\end{bmatrix}}_{\bm{R}} \;.
```

Somit können wir die inneren Stabkräfte durch die externen Lasten $\bm{F}\rs{ext}$ und Reaktionskräfte $\bm{R}$ ersetzen:

```{math}
:label: stabFEM9
\begin{align}
\begin{bmatrix}
F \\
0 \\
R
\end{bmatrix} + \frac{1}{4}n\ell A \begin{bmatrix}
1 \\
1+1\\
1
\end{bmatrix} = \frac{2EA}{\ell}\begin{bmatrix}
1 & -1 & 0\\
-1 & 2 & -1\\
0 & -1 & 1
\end{bmatrix} \begin{bmatrix}
u_1 \\
u_2\\
u_3
\end{bmatrix}
\end{align} \; .
```

```{admonition} Direkte Assemblierung
:class: tip
Innerhalb einer FE-Software werden die entsprechenden Matrizen natürlich nicht explizit zunächst auf die Größe der globalen Matrix "aufgeblasen" um sie später zu addieren. Über die Zuordnung von lokaler zur globalen Knotennummer kann dies effizient direkt erfolgen.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


### Lösung des Gleichungssystems

In der vorliegenden Form ist die Systemsteifigkeitsmatrix singulär und daher kann das lineare Gleichungssystem so nicht gelöst werden. Das bedeutet aus physikalischer Sicht, dass das System in der Lage ist, Starrkörperbewegungen durchzuführen. Um dieses Problem zu beheben, müssen die kinematischen Randbedingungen integriert werden. Dies geschieht durch Partitionierung des linearen Gleichungssystems. Dabei ordnen wir die Verschiebungsgrößen in bekannte $\bar{\bm{u}}$ und unbekannte $\bm{u}$ Größen:

```{math}
:label: LGSsolve1
\begin{bmatrix}
\bm{K}_{uu} & \bm{K}_{u\bar{u}} \\
\bm{K}_{\bar{u}u} & \bm{K}_{\bar{u}\bar{u}}
\end{bmatrix}
\begin{bmatrix}
\bm{u} \\
\bar{\bm{u}}
\end{bmatrix} = \begin{bmatrix} \bm{F}\rs{ext} \\ \bm{R} \end{bmatrix} + \bm{F}_v
```
Hierbei repräsentiert $\bm{F}\rs{ext}$ die einwirkenden Knotenkräfte und $\bm{R}$ die Reaktionskräfte, die den vorgeschriebenen Verschiebungsgrößen zugeordnet sind. Nun lässt sich die erste Zeile nach den unbekannten Verschiebungen auflösen:

```{math}
:label: LGSsolve2
\begin{bmatrix}
\bm{K}_{uu}
\end{bmatrix}
\begin{bmatrix}
\bm{u}
\end{bmatrix} = \begin{bmatrix} \bm{F}\rs{ext}\end{bmatrix} + \bm{F}_v - \begin{bmatrix}
\bm{K}_{u\bar{u}} 
\end{bmatrix} \begin{bmatrix}
\bar{\bm{u}}
\end{bmatrix}
```
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Mit der Lösung dieses linearen Gleichungssystems erhält man die unbekannten Knotenverschiebungen. Anschließend können die unbekannten Reaktionskräfte einfach durch Matrizenmultiplikation bestimmt werden:


```{math}
:label: LGSsolve3
\begin{bmatrix} \bm{R} \end{bmatrix} = 
\begin{bmatrix}
\bm{K}_{\bar{u}u}
\end{bmatrix}
\begin{bmatrix}
\bm{u}
\end{bmatrix} + \begin{bmatrix}
\bm{K}_{\bar{u}\bar{u}} 
\end{bmatrix} \begin{bmatrix}
\bar{\bm{u}}
\end{bmatrix}- \bm{F}_v
```


+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Im vorliegenden Beispiel erhalten wir für das Gleichungssystem {eq}`LGSsolve2`:

\begin{align}
\begin{bmatrix}
F \\
0 \\
\end{bmatrix} + \frac{1}{4}n\ell A \begin{bmatrix}
1 \\
1+1
\end{bmatrix} - \frac{2EA}{\ell}\begin{bmatrix}
 0\\
-1
\end{bmatrix} \begin{bmatrix}
0
\end{bmatrix} & = \frac{2EA}{\ell}\begin{bmatrix}
1 & -1\\
-1 & 2 \\
\end{bmatrix} \begin{bmatrix}
u_1 \\
u_2\\
\end{bmatrix} \\
\begin{bmatrix}
F +\frac{1}{4}n\ell A \\
\frac{1}{2}n\ell A \\
\end{bmatrix} & =
\frac{2EA}{\ell}\begin{bmatrix}
1 & -1\\
-1 & 2 \\
\end{bmatrix} \begin{bmatrix}
u_1 \\
u_2\\
\end{bmatrix} 
\end{align}

mit der Lösung:

\begin{align}
\begin{bmatrix}
u_1 \\
u_2\\
\end{bmatrix} = \begin{bmatrix}
 \frac{F\ell}{EA} + \frac{n \ell^2}{2E}\\
\frac{F\ell}{2EA} + \frac{3 n \ell^2}{8E}
\end{bmatrix} 
\end{align}

Die unbekannte Reaktionskraft am Knoten 3 berechnet sich dann zu:

\begin{align}
R + F_v & = \frac{2EA}{\ell} \begin{bmatrix}
0 & -1 
\end{bmatrix} \begin{bmatrix}
u_1 \\ u_2
\end{bmatrix} + \begin{bmatrix}
1
\end{bmatrix} \begin{bmatrix}
0
\end{bmatrix} \\
&= -\frac{2EA}{\ell} \left(\frac{F\ell}{2EA} + \frac{3 n \ell^2}{8E} \right)- \frac{1}{4}n\ell A= -F - n \ell A 
\end{align}

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


### Berechnung der Schnittgrößen

Die Schnittgrößen $N^{(i)}=\sigma^{(i)} A$ werden in einer Nachlaufrechnung (Postprocessing) mittels des Materialmodell $\sigma^{(i)}=E\e^{(i)}$ bestimmt. Dabei sind die Schnittkräfte:



```{math}
:label: postproc
\begin{align}
N^{(1)} & = 2\frac{EA}{\ell} \begin{bmatrix} -1 & - \end{bmatrix} \begin{bmatrix} u_3 \\ u_2 \end{bmatrix} = F + \frac{3}{4} n A \ell \\
N^{(2)} & = 2\frac{EA}{\ell} \begin{bmatrix} -1 & 1 \end{bmatrix} \begin{bmatrix} u_2 \\ u_1 \end{bmatrix}= F + \frac{1}{4} n A \ell
\end{align}
```


+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Diskussion der Ergebnisse

```{code-cell} ipython3
---
mystnb:
  figure:
    caption: |
      Vergleich der analytischen Lösung mit der FE-Lösung.
    name: FE-compare
tags: [
  "remove-input"
  ]
---
import numpy as np
import matplotlib.pyplot as plt

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

sizex = 7.5
sizey = 5

# Create a figure and axis.
fig, ax = plt.subplots(1, 2,figsize=(2*sizex,sizey))

F=1
E=1
A=1
l=20
n=1

x = np.linspace(0, l, 100)


# Plotting the functions.
ax[0].plot(x, (F/(E*A)+n*l/E)*x-(n/(2*E))*x**2,color='red',linewidth=2.0,label="analytische Lösung")

fem_sol=np.array([
 [0,0],
 [l/2,F*l/(2*E*A)+3*n*l**2/(8*E)],
 [l,F*l/(E*A)+n*l**2 / (2*E)]   
])

#ax[0].scatter(fem_sol[:,0],fem_sol[:,1],color="blue",marker="o",s=100,label="FEM")
ax[0].plot(fem_sol[:,0],fem_sol[:,1],color="blue",marker="o",markersize=5,label="FEM")
# Set labels for the axis
ax[0].set_xlabel(r"$x$")
ax[0].set_ylabel(r"$u(x)$")
ax[0].set_title("Verschiebungsverlauf")

# Set custom ticks for the x-axis with LaTeX symbols
tick_positions = [0, l/2, l]
tick_labels = [r"$0$", r"$\frac{\ell}{2}$", r"$\ell$"]
ax[0].set_xticks(tick_positions)
ax[0].set_xticklabels(tick_labels)
ax[0].set_yticks([0,F*l/(2*E*A)+3*n*l**2/(8*E),F*l/(E*A)+n*l**2 / (2*E)])
ax[0].set_yticklabels([r"$0$",r"$\frac{F\ell}{EA} + \frac{n \ell^2}{2E}$",r"$\frac{F\ell}{2EA} +\frac{3 n \ell^2}{8E}$"])

# Draw background grid
ax[0].grid(True)

# Add a second subplot (you can customize this as needed)
# For demonstration, let's just add a simple plot in the second subplot
fem_sol_N=np.array([F+3/4*n*A*l,F+1/4*n*A*l])
ax[1].plot(x, F+n*A*(l-x), color='red', linewidth=2.0)
ax[1].plot([0,l/2],[fem_sol_N[0],fem_sol_N[0]],color='blue',linewidth=2.0)
ax[1].plot([l/2,l],[fem_sol_N[1],fem_sol_N[1]],color='blue',linewidth=2.0)
ax[1].set_xlabel(r"$x$")
ax[1].set_ylabel(r"$N(x)$")
ax[1].set_title("Schnittkraftverlauf")
ax[1].grid(True)
ax[1].set_xticks(tick_positions)
ax[1].set_xticklabels(tick_labels)

ax[1].set_yticks([1,F+n*l/2*A,F+n*l])
ax[1].set_yticklabels([r"$F$",r"$F+\frac{nA\ell}{2}$",r"$F+nA\ell$"])

# Create a common legend below both subplots
fig.legend(loc='lower center', bbox_to_anchor=(0.5, -0.08), ncol=2)

# Adjust layout to make room for the legend
#plt.tight_layout(rect=[0, 0.1, 1, 1])  # Adjust the rect to make space for the legend

# Show the plot
plt.show()

# Add legend.
# ax[0].legend([r"analytische Lösung", r"FEM"])

```


Abbildung {numref}`FE-compare` links zeigt einen Vergleich zwischen dem mit der Finite-Elemente-Methode (FEM) ermittelten Verschiebungsfeld und der analytischen Lösung. Letztere zeigt aufgrund der Eigengewichtsbelastung einen quadratischen Verlauf, während die FEM diesen in Abschnitten durch lineare Näherungen darstellt. In diesem speziellen Fall ist erkennbar, dass die FEM die analytische Lösung an den Knotenpunkten exakt wiedergibt. 

Im rechten Teil von Abbildung {numref}`FE-compare` wird die FE-Approximation für den Verlauf der Schnittkräfte mit der analytischen Lösung kontrastiert. Die analytische Lösung zeigt ein lineares Wachstum der Normalkraft auf, das jedoch durch die hier verwendeten linearen Ansatzfunktionen lediglich stückweise durch eine konstante Annäherung wiedergegeben wird. Dabei treten an den Grenzen der Elemente Diskontinuitäten in den von der FEM berechneten Schnittkraftverläufen auf. Auffällig ist auch in diesem Beispiel, dass die analytische Lösung im Zentrum jedes Elements exakt erreicht wird.

Die Genauigkeit dieser Näherungslösung kann augenscheinlich gesteigert werden, indem man die Anzahl der Elemente erhöht. Dies führt dazu, dass die Sprünge im Verlauf der Schnittkräfte bestehen bleiben, aber kleiner ausfallen. Um eine qualitativ signifikante Verbesserung zu erzielen, empfiehlt es sich, Elemente mit quadratischen Ansatzfunktionen für das Verschiebungsfeld zu nutzen. In diesem Fall wird bereits mit einem einzigen Element, das über drei Knotenpunkte verfügt, die exakte Lösung erlangt. In dieser Situation wäre dann die FEM äquivalent dem Ritz'schen Verfahren.

### Konvergenz der Ergebnisse

```{figure} images/Stab_Normalkraft_refined.png
---
width: 600px
name: stabelement_refined
---
Ergebnisse für die Normalspannung in einem Stab unter Eigengewicht mit einer erhöhten Netzfeinheit.
```

Bei einer immer feiner werdenen Diskretisierung des Stabes, konvergiert die Lösung gegen die analytische Lösung. Dennoch liegen wie in Abbildung {numref}`stabelement_refined` zu sehen, immernoch Spannungssprünge an den Elementgrenzen vor. Betrachtet man die Fehler der Spannungsergebnisse, so zeigt sich, dass dieser Fehler mit steigender Anzahl an Freihheitsgraden abnimmt. Dies ist in Abbildung {numref}`stabelement_error` zu sehen. Im doppelt logaritmischen Maßstab erhält man eine Gerade für die Abnahme des Fehlers. Auch bei der verwendung von Ansatzfunktionen höherer Ordnung reduziert sich der Fehler nur linear im doppeltlogarithmischen Maßstab. Die Steigung ist jedoch höher, sodass im Allgemeinen mit einer verbesserten Konvergenz zu rechnen ist.


```{figure} images/Stab_error_convergence.png
---
width: 600px
name: stabelement_error
---
Konvergenz der Spannungsergebniss in einem Stab unter Eigengewicht mit steigender Anzahl an Freihheitsgraden.
```

## Interactives Notebook

asierend auf der präsentierten Theorie wurde ein Jupyter Notebook erstellt. Dieses kann man ohne Systemvorraussetzungen im Browser ausführen:

StabFEM for Binder: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/steffenbeese/FEM_I_Notebooks/main?urlpath=%2Fdoc%2Ftree%2FNotebook_StabFEM.ipynb)



```{admonition} Fragen zum Kapitel
:class: warning

**StabFEM**

- Was unterscheidet das Verfahren von Ritz von der FEM?
- Welches Problem hat man beim Verfahren von Ritz bezüglich der Anssatzfunktion?
- Skizzieren Sie die Formfunktionen für ein lineares Stabelement mit 2 Knoten.
- Auf Elementebene werden die Steifigkeitsmatrizen und die Lastvektoren berechnet. Was passiert danach mit diesen Größen?
- Das globale Gleichungssystem kann nicht gelöst werden. Die Steifigkeitsmatrix ist singulär. Was könnte der Grund dafür sein?

```