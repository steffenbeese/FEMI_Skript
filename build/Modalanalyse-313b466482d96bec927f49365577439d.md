---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.17.0
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---
+++ {"editable": true, "slideshow": {"slide_type": ""}}

# Modalanalysen

Die Modalanalyse beschäftigt sich mit den Eigenschwingungen eines mechanischen Systems. Das Ziel der Modalanalyse ist es, die Eigenfrequenzen und Eigenformen (**Eigenmoden**) des Systems zu berechnen. Wichtig ist es an dieser Stelle darauf hinzuweisen, dass alle hier beschriebenen Methoden nur für lineare Systeme gelten.

```{figure} images/Takoma_bridge.png
---
name: Takoma_Bridge
alt: Eigenschwingung der Takoma Bridge
width: 500px
---
Eigenschwingungen der Takoma Bridge bis zum Versagen. [wikipedia](https://de.wikipedia.org/wiki/Tacoma-Narrows-Br\%C3\%BCcke)
```

Grundlage für die Modalanalyse ist die Impulsbilanz (Bewegungsgleichung) {eq}`FEM_dynamisch`:

\begin{equation*}
\bm{M} \ddot{\bm{u}} + \bm{C} \dot{\bm{u}} + \bm{K} \bm{u} = \bm{f}(t) \; .
\end{equation*}

Als Unbekannte für die Bewegungsgleichung sind für alle Punkte $\bm{x}$ im Raum und das betrachtete Zeitinterval $[t_0, t_1]$ die Verschiebungen $\bm{u}(\bm{x}, t)$, die Geschwindigkeiten $\dot{\bm{u}}(\bm{x}, t)$ und die Beschleunigungen $\ddot{\bm{u}}(\bm{x}, t)$ gesucht.

Bei der Modalanalyse wird davon ausgegangen, dass die Lösung der Impulsbilanz {eq}`FEM_dynamisch` eine harmonische Funktion ist:

```{math}
:label: AnsatzModal
\bm{u}(\bm{x}, t) = \underbrace{A}_{Amplitude} \sin(\underbrace{\omega}_{Eigenfrequenz} t + \underbrace{\theta}_{Phase} )
```

Die Lösung $\bm{u}$ der Impulsbilanz wird folglich interpretiert wie ein 1D-Massenschwinger unter einer harmonischen Anregung.

```{figure} images/spring.png
---
name: Massenschwinger
alt: 1D Massenschwinger
width: 500px
---
Eindimensionaler Feder-Massenschwinger mit Dämpfung.
```

```{code-cell}
---
mystnb:
  figure:
    caption: "Position der Masse m in Abh\xE4ngigkeit von der Zeit.\n"
    name: harmonischeAnregung
tags: [hide-input]
---
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import Circle
import scienceplots

# Set up the plot configurations.
plt.style.use(['science', 'grid'])
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

t = np.linspace(0, 2*np.pi, 200)

A = 1
omega = 1
phi = np.pi/3

fig,ax = plt.subplots(1,1,figsize=(10,5));

ax.plot(t, A*np.sin(omega*t+phi), label=r'$x(t) = A \sin(\omega t + \theta)$'),
ax.set_xlabel(r'$t$');
ax.set_ylabel(r'$x(t)$');
ax.legend();

ax.annotate(r'Amplitude $A$', xy=(-phi + 0.5*np.pi*omega, A), xytext=(0.8, 1.2*A),
            arrowprops=dict(facecolor='red',edgecolor='red', shrink=0.05), fontsize=12);

ax.axvline(0, color='grey', linestyle='--')
ax.axvline(-phi+0.5*np.pi*omega, color='grey', linestyle='--');
ax.annotate('', xy=(0, 0), xytext=(-phi+0.5*np.pi*omega, 0),
            arrowprops=dict(arrowstyle='<->', color='black', lw=1.5));

ax.annotate(r'$\theta$', xy=(-phi+0.4*np.pi*omega, 0.1*A), fontsize=12);
```

Man kann sich das Verhalten der Struktur so vorstellen, als ob jeder Knoten des Systems als ein Feder-Masse-System interpretiert werden kann. Jede Masse schwingt mit der gleichen Frequenz und Phase, aber mit einer anderen Amplitude.

```{figure} images/Modal_Ansys_01.png
---
name: ModalAnsys01
alt: ModalinterpretationAnsys
width: 500px
---
Darstellung der Struktur als System von Massenschwingern [Ansys Innovation Space](https://innovationspace.ansys.com/courses/courses/modal-analysis/lessons/governing-equations-of-modal-analysis-lesson-2/)
```

Da die Dämpfungsmatrix $\mathbf{C}$ für gewöhnlich aus dem Materialverhalten resultiert und bei der Modalanalyse lediglich linear elastisches Materialverhalten betrachtet wird, wird diese Matrix   vernachlässigt. Weiterhin sind die Eigenformen unabhängig von der äußeren Belastung, so dass die resultierende Bewegungsgleichung:

\begin{equation}
\bm{M}\ddot{\bm{u}} + \bm{K}\bm{u} = \bm{0}
\end{equation}
lautet. Wird nun der Ansatz der Verschiebung $\bm{u}$ und der Beschleunigung $\ddot{\bm{u}}$ eingesetzt:

```{math}
:label: FEMLinearDynamics
\begin{align}
 \bm{u}(t) & = \bm{A} \sin\left(\omega t + \theta\right) \\
 \ddot{\bm{u}}(t) & = -\omega^2 \bm{A} \sin\left(\omega t + \theta\right) \; ,
\end{align}
```

so ergibt sich daa Gleichungssystem:

\begin{equation}
\left( \bm{K} - \omega^2 \bm{M} \right) \bm{\phi} = \bm{0} \; .
\end{equation}

Dies ist ein klassisches Eigenwertproblem mit den Eigenwerten $\omega_i^2$ und den Eigenvektoren $\bm{\phi}_i$.

```{admonition}
:class: dropdown

Stellen Sie die Bewegungsgleichungen für das dargestellte System auf und bestimmen Sie die Eigenfrequenzen und Eigenvektoren.

**gegeben:** $m,\,k,\,m_1=3m,\,m_2=m,\,k_1=k,\,k_2=2k,\,k_3=3k$

![2DMassenschwinger](./images/spring_2.png)


**Lösung:** 

\begin{align*}
\omega_1^2 &= \frac{k \left(3 - \frac{4 \sqrt{3}}{3}\right)}{m}\\
\omega_2^2 &=\frac{k \left(\frac{4 \sqrt{3}}{3} + 3\right)}{m}
\end{align*}


\begin{align*}
\bm{\phi}_1 &= \left[\begin{matrix}1 + \frac{2 \sqrt{3}}{3}\\1\end{matrix}\right]
\\
\bm{\phi}_2 &=  \left[\begin{matrix}1 - \frac{2 \sqrt{3}}{3}\\1\end{matrix}\right]
\end{align*}


```

## Modanalyse in der FEM

Die Bestimmung der Eigenfrequenzen und Moden basiert auf Gleichung {eq}`FEMLinearDynamics` am diskretisierten FE-System. Dies bedeutet gleichzeitig, dass man genauso viele Moden und Eigenfrequenzen findet wie es Freiheitsgrade im System gibt. Sind durch die Randbedingungen Starrkörperbewegungen nicht unterdrückt ergeben sich "0"-Eigenfrequenzen und Moden. Deshalb ist es wichtig auf die Unverschieblichkeit des Systems zu achten. Die Berechnung aller Eigenfrequenzen und -moden ist ressourcenintensiv und im allgemeinen nicht zielführend. Man stelle sich zum Beispiel die Stimmgabel in Abbildung vor:

```{figure} images/Stimmgabel.png
---
name: Stimmgabel
alt: Modell einer Stimmgabel
width: 500px
---
Modell einer Stimmgabel in Ansys [Ansys Learning Resources](https://www.ansys.com/academic/educators/education-resources/tuning-fork-frequency-ansys-discovery). 
```

Hier sind wir nur an den kleinsten Eigenfrequenzen interessiert. Das dargestellte Modell sollte eine natürliche Frequenz von 512 Hz liefern - den Ton C.

Für die dargestellte Struktur liefert Ansys folgende 6 ersten Eigenfrequenzen:

| Mode | Eigenfrequenz in Hz |
| ---- | ------------------- |
| 1    | 306,16              |
| 2    | 318,05              |
| 3    | 511,91              |
| 4    | 536,35              |
| 5    | 2087,6              |
| 6    | 2113,5              |

```{figure} images/Stimmgabel_Mode.png
---
name: StimmgabelMode
alt: Moden der Stimmgabel
width: 800px
---
Ersten drei Eigenmoden der Stimmgabel. Mode 3 gehört zur gesuchten Frequenz von 512 Hz.
```

Die Frequenzen werden direkt in Hz angegeben, es handelt sich also um die Werte:

\begin{equation}
 f_i = \frac{\sqrt{\omega_i^2}}{2\pi} \, .
\end{equation}

Die so berechneten Eigenfrequenzen werden übrigens nicht mit einem direkten Verfahren gelöst, sondern mit einem Iterativen (Vektoriteration oder Lanczos-Iteration). Weiterführende Informationen hierzu kann man zum Beispiel im Buch von H. R. Schwarz {cite}`schwarz2013methode`.

Als zusätzliches Ergebnis der Modalanalyse erhält man im Analyse-File auch die effektiven Massen $m_{\text{eff},i}$:

\begin{equation}
m_{\text{eff},i} = \left[  \bm{\phi}^T \bm{M} \bm{D}\right]^2 \; .
\end{equation}

Hierbei ist $\bm{M}$ wieder die Massenmatrix und $\bm{D}$ ein Vektor, welcher für jeden Knoten die Richtung enthält bezüglich derer man die effektive Masse bestimmen möchte. Im Analyse-file wird dies wie folgt angegeben:

```
          ***** PARTICIPATION FACTOR CALCULATION *****  Y  DIRECTION
                                                                                  CUMULATIVE     RATIO EFF.MASS
  MODE   FREQUENCY       PERIOD      PARTIC.FACTOR     RATIO    EFFECTIVE MASS   MASS FRACTION   TO TOTAL MASS
     1     306.162       0.32662E-02   0.72527E-02    1.000000    0.526020E-04    0.784215        0.598442  
     2     318.052       0.31441E-02   0.16450E-07    0.000002    0.270589E-15    0.784215        0.307844E-11
     3     511.911       0.19535E-02   0.52910E-05    0.000730    0.279943E-10    0.784215        0.318486E-06
     4     536.349       0.18645E-02  -0.20278E-08    0.000000    0.411180E-17    0.784215        0.467792E-13
     5     2087.59       0.47902E-03  -0.73383E-07    0.000010    0.538513E-14    0.784215        0.612656E-10
     6     2113.47       0.47316E-03  -0.38045E-02    0.524558    0.144740E-04     1.00000        0.164668  
 -----------------------------------------------------------------------------------------------------------------
```

Die effektive Masse in y-Richtung für Mode 1 gibt an, wie viel Masse bei Mode 1 in y-Richtung bewegt wird. Mode 1,6 und 3 sind maßgeblich für die Bewegung in y-Richtung.

```{admonition}
:class: dropdown

Für das dargestellte System haben wir bereits die Eigenwerte und Eigenvektoren bestimmt. 

**gegeben:** $m,\,k,\,m_1=3m,\,m_2=m,\,k_1=k,\,k_2=2k,\,k_3=3k$

![2DMassenschwinger](./images/spring_2.png)

Nun möchten wir die spezifischen Massen $m_{eff,1}$ und $m_{eff,2}$ bestimmen. 

**Lösung:** 

Wir bestimmen zunächst die Anteilsfaktoren $\gamma_1$ und $\gamma_2$:

\begin{align*}
\gamma_1 &= \bm{\phi_1}^T \bm{M} \bm{D} =  \begin{bmatrix}1 + \frac{2 \sqrt{3}}{3}& 1\end{bmatrix} \left[\begin{matrix}3 m & 0\\0 & m\end{matrix}\right]  \begin{bmatrix} 1 \\ 1\end{bmatrix} = 2 m \left(2+ \sqrt{3}  \right)\\
\gamma_2 &= \bm{\phi_2}^T \bm{M} \bm{D} =  \begin{bmatrix}1 - \frac{2 \sqrt{3}}{3}\\1\end{bmatrix} \left[\begin{matrix}3 m & 0\\0 & m\end{matrix}\right]  \begin{bmatrix} 1 \\ 1\end{bmatrix} = 2 m \left(2 - \sqrt{3}\right)
\end{align*}


Mit der Beziehung: $m_{eff,i} = \frac{\gamma_i^2}{\bm{\phi}^T\bm{M}\bm{\phi}}$ können wir dann die effektiven Massen berechnen:

\begin{align*}
m_{eff,1} &= \frac{4 m^2 \left(2+ \sqrt{3}  \right)^2}{\begin{bmatrix}1 + \frac{2 \sqrt{3}}{3}& 1\end{bmatrix} \left[\begin{matrix}3 m & 0\\0 & m\end{matrix}\right] \begin{bmatrix}1 + \frac{2 \sqrt{3}}{3}& 1\end{bmatrix}} = 2 m + \sqrt{3} m 
\\
m_{eff,2} &= \frac{ 4 m^2 \left(2 - \sqrt{3}\right)^2 }{\begin{bmatrix}1 - \frac{2 \sqrt{3}}{3} & 1\end{bmatrix} \left[\begin{matrix}3 m & 0\\0 & m\end{matrix}\right] \begin{bmatrix}1 - \frac{2 \sqrt{3}}{3}\\1\end{bmatrix}} = 2 m - \sqrt{3} m  
\end{align*}

Da es nur zwei Moden in diesem System gibt, ist die Summe der effektiven Massen $m_{eff,1}$ und $m_{eff,2}$ gleich der Gesamtmasse des Systems:

\begin{equation}
m_{eff,1}+ m_{eff,2} = 4m \; .
\end{equation}

Bestimmt man nun noch die relative Masse $m_{rel,i}=\frac{m_{eff,i}}{m_{gesamt}}$, so erhält man eine Aussage darüber welcher Anteil der Masse bei dem entsprechenden Mode angeregt wird:

| Mode | relative effektive Masse |
| -----| ------------------------ |
|   1  | 0.93                     |
|   2  | 0.067                    |
| Summe| 1                        |


```

## Modenüberlagerung in Transienter Berechnung

Wenn nicht nur die Eigenfrequenzen und Eigenformen eines Systems von Bedeutung sind, sondern auch eine Analyse bzgl. einer harmonischen Anregung, einer zufallsverteilten Vibration oder generell einer zeitabhängigen Kraft, kann man über die Modenüberlagerung effektiv eine Lösung finden.

Die Grundidee der Modenüberlagerung ist die Darstellung des Verschiebungsansatzes:

```{math}
:label: msupansatz
\begin{equation}
\bm{u}_h = \sum_i^n \bm{\phi}_i y_i
\end{equation}
```

Dabei kommen nicht alle möglichen Moden zum Einsatz, sondern nur ein reduzierter Satz von $m \ll n$ Moden, wobei $n$ die Anzahl der Freiheitsgrade des Systems ist. In Matrixschreibweise lässt sich der Ansatz kompakt formulieren als:

```{math}
:label: msupmatrix
\begin{equation}
\bm{u}_h = \bm{\Phi} \, \bm{y}(t)
\end{equation}
```

mit der Modalmatrix $\bm{\Phi} = [\bm{\phi}_1, \bm{\phi}_2, \dots, \bm{\phi}_m] \in \mathbb{R}^{n \times m}$ und dem Vektor der modalen Koordinaten $\bm{y}(t) = [y_1(t), y_2(t), \dots, y_m(t)]^T$. Entsprechend ergeben sich die Zeitableitungen:

\begin{align}
\dot{\bm{u}}_h &= \bm{\Phi} \, \dot{\bm{y}}(t) \\
\ddot{\bm{u}}_h &= \bm{\Phi} \, \ddot{\bm{y}}(t) \; .
\end{align}

### Einsetzen in die Bewegungsgleichung

Ausgangspunkt ist die vollständige FEM-Bewegungsgleichung {eq}`FEM_dynamisch`:

\begin{equation}
\bm{M} \ddot{\bm{u}} + \bm{C} \dot{\bm{u}} + \bm{K} \bm{u} = \bm{f}(t) \; .
\end{equation}

Einsetzen des Ansatzes {eq}`msupmatrix` liefert:

```{math}
:label: msup_eingesetzt
\begin{equation}
\bm{M} \bm{\Phi} \, \ddot{\bm{y}} + \bm{C} \bm{\Phi} \, \dot{\bm{y}} + \bm{K} \bm{\Phi} \, \bm{y} = \bm{f}(t) \; .
\end{equation}
```

Dieses Gleichungssystem hat $n$ Gleichungen, aber nur $m$ Unbekannte. Um ein bestimmtes System zu erhalten, wird {eq}`msup_eingesetzt` von links mit $\bm{\Phi}^T$ multipliziert (Galerkin-Projektion):

```{math}
:label: msup_projiziert
\begin{equation}
\underbrace{\bm{\Phi}^T \bm{M} \bm{\Phi}}_{\tilde{\bm{M}}} \, \ddot{\bm{y}} + \underbrace{\bm{\Phi}^T \bm{C} \bm{\Phi}}_{\tilde{\bm{C}}} \, \dot{\bm{y}} + \underbrace{\bm{\Phi}^T \bm{K} \bm{\Phi}}_{\tilde{\bm{K}}} \, \bm{y} = \underbrace{\bm{\Phi}^T \bm{f}(t)}_{\tilde{\bm{f}}(t)} \; .
\end{equation}
```

Das resultierende System hat nun nur noch die Dimension $m \times m$ anstatt $n \times n$.

### Orthogonalität der Eigenvektoren

Die Eigenvektoren $\bm{\phi}_i$ besitzen die wichtige Eigenschaft der **Orthogonalität** bezüglich der Massen- und Steifigkeitsmatrix:

```{math}
:label: orthogonalitaet
\begin{align}
\bm{\phi}_i^T \bm{M} \bm{\phi}_j &= 0 \quad \text{für } i \neq j \\
\bm{\phi}_i^T \bm{K} \bm{\phi}_j &= 0 \quad \text{für } i \neq j \; .
\end{align}
```

Dadurch werden die projizierten Matrizen $\tilde{\bm{M}}$ und $\tilde{\bm{K}}$ zu **Diagonalmatrizen**:

\begin{align}
\tilde{\bm{M}} = \bm{\Phi}^T \bm{M} \bm{\Phi} &= \text{diag}(\tilde{m}_1, \tilde{m}_2, \dots, \tilde{m}_m) \\
\tilde{\bm{K}} = \bm{\Phi}^T \bm{K} \bm{\Phi} &= \text{diag}(\tilde{k}_1, \tilde{k}_2, \dots, \tilde{k}_m)
\end{align}

mit den modalen Massen $\tilde{m}_i = \bm{\phi}_i^T \bm{M} \bm{\phi}_i$ und den modalen Steifigkeiten $\tilde{k}_i = \bm{\phi}_i^T \bm{K} \bm{\phi}_i = \omega_i^2 \tilde{m}_i$.

Nimmt man zusätzlich eine **Rayleigh-Dämpfung** an, d.h. $\bm{C} = \alpha \bm{M} + \beta \bm{K}$, so ist auch $\tilde{\bm{C}}$ eine Diagonalmatrix, da:

\begin{equation}
\tilde{\bm{C}} = \bm{\Phi}^T \bm{C} \bm{\Phi} = \alpha \, \tilde{\bm{M}} + \beta \, \tilde{\bm{K}} = \text{diag}(\tilde{c}_1, \tilde{c}_2, \dots, \tilde{c}_m) \; .
\end{equation}

### Entkoppelte modale Gleichungen

Durch die Diagonalstruktur aller drei Matrizen **entkoppelt** das System {eq}`msup_projiziert` vollständig in $m$ unabhängige skalare Differentialgleichungen:

```{math}
:label: msup_entkoppelt
\begin{equation}
\tilde{m}_i \, \ddot{y}_i + \tilde{c}_i \, \dot{y}_i + \tilde{k}_i \, y_i = \tilde{f}_i(t) \quad \text{für } i = 1, 2, \dots, m
\end{equation}
```

mit der modalen Kraft $\tilde{f}_i(t) = \bm{\phi}_i^T \bm{f}(t)$. Division durch die modale Masse $\tilde{m}_i$ ergibt die Standardform:

```{math}
:label: msup_standard
\begin{equation}
\ddot{y}_i + 2 \zeta_i \omega_i \, \dot{y}_i + \omega_i^2 \, y_i = \frac{\tilde{f}_i(t)}{\tilde{m}_i}
\end{equation}
```

mit dem modalen Dämpfungsgrad $\zeta_i = \frac{\tilde{c}_i}{2 \omega_i \tilde{m}_i}$. Jede dieser Gleichungen entspricht einem gedämpften Einmassenschwinger und kann unabhängig von den anderen zeitlich integriert werden.

```{admonition} Zusammenfassung des Verfahrens
:class: tip

1. **Modalanalyse:** Löse das Eigenwertproblem $(\bm{K} - \omega_i^2 \bm{M})\bm{\phi}_i = \bm{0}$ und bestimme die $m$ relevanten Eigenmoden.
2. **Projektion:** Berechne die modalen Größen $\tilde{m}_i$, $\tilde{k}_i$, $\tilde{c}_i$ und $\tilde{f}_i(t)$.
3. **Integration:** Löse die $m$ entkoppelten skalaren Gleichungen {eq}`msup_standard` nach $y_i(t)$.
4. **Rücktransformation:** Berechne die physikalischen Verschiebungen über $\bm{u}_h(t) = \sum_i^m \bm{\phi}_i \, y_i(t)$.

Anstatt ein gekoppeltes System mit $n$ Freiheitsgraden zu lösen, werden nur $m \ll n$ skalare Gleichungen integriert. Der Rechenaufwand reduziert sich damit drastisch.
```

Details zum Verfahren findet man in dem Lehrwerk {cite}`bathe2006finite`.



Die Modenüberlagerung ist nachfolgend am Beispiel eines Sprungbrettes gezeigt und wird mit der vollen transienten Lösung verglichen. Siehe hierzu [Transiente Berechnung](./TransienteDynamik.md#beispiel-sprungbrett-mit-impliziter-zeitintegration)

```{figure} images/Vergleich_MSUP_NM.png
---
name: Vergleich_MSUP_NM
alt: Vergleich der Modenüberlagerung mit transienter Berechnung
width: 700px
---
Vergleich der Modenüberlagerung mit der transienten Berechnung.
```

## Modenüberlagerung in Harmonischer Berechnung

Die Modenüberlagerungstechnik wird nicht nur für die transiente Berechnung verwendet, sondern kann auch für eine harmonische Berechnung genutzt werden. Harmonische Berechnungen liegen vor, wenn eine oder mehrere sinusförmige Kräfte $F_i= \hat{F}_i \sin(\omega t+ \phi_i)$ mit gleicher Frequenz $\omega$ angeregt werden. Die Lösung der harmonischen Bewegungsgleichung liefert dann die Amplitude der Verschiebung $u$ und die Phase $\phi_i$ über der Frequenz $\omega$. Anstatt die volle harmonische Bewegungsgleichung zu lösen kann hier wieder die Modenüberlagerung eingesetzt werden.

### Beispiel: Zwei Kompressoren auf einem Rahmen

```{figure} images/Kompressoren.png
--- 
name: Kompressoren
alt: Kompressoren mit harmonischer Anregung
width: 500px
---
Darstellung des Kompressorenmodells und der harmonischen Anregung.
```

Zwei Kompressoren werden auf einem Rahmen montiert. Idealisiert werden die beiden Kompressoren durch Quader dargestellt. Dies ist auch für praktische Fragestellungen möglich. Jedoch sollte man darauf achten, dass die Trägheitseigenschaften der Kompressoren nicht zu stark von den Trägheitseigenschaften des Ersatzmodells abweichen.
Am rechten Kompressor greift eine Harmonische Kraft in vertikaler Richtung an. Gelöst wird diese Problemstellung zum einen mit einer vollen harmonischen Berechnung und mit der Modenüberlagerungstechnik mit 10 Basismoden. Bei der vollen harmonischen Berechnung wird das gesamte System bei jeder untersuchten Frequenz vollständig gelöst. Hingegen bei der Modenüberlagerungstechnik wird nur die Approximation gelöst, welches einen deutlich geringeren Rechenaufwand darstellt. Das Ergebnis der Frequenzanalyse ist in Abbildung {numref}`Frequenzgang` dargestellt.

```{figure} images/Frequenzgang_harmonsicher_Anregung.png
---
name: Frequenzgang
alt: Frequenzgang der Verschiebung bei einer harmonischen Anregung
width: 700px
---
Frequenzgang der vertikalen Verschiebung eines Kompressors bei harmonischer Anregung.
```

Die größten Verschiebungen ergeben sich bei der Frequenz von 40 Hz auf.

Die kritischen Frequenzen werden auch bei der Modenüberlagerungstechnik gut erfasst. Die Amplituden der Resonanzen sind jedoch etwas kleiner als bei der vollen harmonischen Berechnung. Dies liegt vor allen an der reduzierten Anzahl der Basismoden.

### Vor- und Nachteile der Modalen Superposition

| Vorteile | Nachteile |
| -------- | --------- |
| Deutlich geringerer Rechenaufwand durch Reduktion auf $m \ll n$ Freiheitsgrade | Nur für **lineare** Systeme anwendbar |
| Entkopplung in unabhängige skalare Gleichungen ermöglicht einfache Zeitintegration | Genauigkeit hängt von der Anzahl der berücksichtigten Moden ab |
| Effizient für transiente und harmonische Berechnungen | Hohe Frequenzanteile werden bei zu wenigen Moden nicht erfasst |
| Physikalische Interpretation der Ergebnisse durch modale Koordinaten | Dämpfung muss speziell modelliert werden (z. B. Rayleigh-Dämpfung) |
| Wiederverwendbarkeit der Modalbasis für verschiedene Lastfälle | Nichtlineare Effekte (Kontakt, Plastizität, große Verformungen) können nicht abgebildet werden |
| Einfache Identifikation dominanter Moden über effektive Massen | Zusätzlicher Aufwand für vorgeschaltete Modalanalyse |

## Zusammenfassung

```{admonition}
:class: tip
- Modalanalyse identifiziert Eigenschwingungen und Eigenformen (Eigenmoden) **linearer**, mechanischer Systeme.
- Klassisches Eigenwertproblem löst Eigenfrequenzen/-formen - wird iterativ gelöst
- Anzahl der möglichen Eigenfrequenzen/-moden durch FE-Diskretisierung gegeben
- Beachtung starre Randbedingungen; Vermeidung "0"-Eigenfrequenzen.
- Modenüberlagerung effektiv für zeitabhängige Analysen (z. B. transiente, harmonische Lasten)
- Verschiebungsansatz reduziert Unbekannte - Nutzung weniger Moden.

```
