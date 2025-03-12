---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.7
---

+++ {"editable": true, "slideshow": {"slide_type": "skip"}}


$
\newcommand{\MtdFull}[1]{\frac{D \, #1}{D \, t}}
\newcommand{\Pd}[2]{\frac{\partial #1}{\partial #2}}
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
$

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


# Von der starken zur schwachen Form

Für die in Kapitel 1 eingeführten Randwertprobleme (Massenbilanz, Impulsbilanz und Energiebilanz) wird in diesem Kapitel die theoretischen Grundlagen der Finite Elemente Methode beschrieben. Für jedes der genannten Probleme ist der Ausgangspunkte die **starke** Form der beschreibenden partiellen Differentialgleichung. Am Beispiel der Impulsbilanz soll exemplarisch die zugehörige **schwache** Form hergeleitet werden. Die Begrifflichkeit stark und schwach bezieht sich hierbei auf die Stetigkeits- und Differenzierbarkeitsanforderungen der gesuchten Lösung $\bm{u}$. Die starke Form hat somit **immer** höhere Anforderungen an die Lösung.

```{figure} ../chapter1/images/Impulsbilanz.jpg
---
height: 400px
name: impulsbilanz
---
Körper $\mathcal{B}$ unter Einwirkung der externen Oberflächenkraft $\tilde{\bm{t}}$ und der Volumenlast $\bm{b}$
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Ausgangspunkt für die exemplarische Herleitung ist die Impulsbilanz in der quasistatischen Form (ohne Trägheitsterme):

```{math}
:label: weakform_01
0 = \div{\bm{\sigma}} + \rho \bm{b} 
```
Diese starke form wird jetzt mit einer beliebigen vektorwertigen Testfunktion $\delta \bm{u}$ multipliziert und über das betrachtete Gebiet $\mathcal{B}$ (den betrachteten Körper) integriert.

```{math}
:label: weakform_02
0 = \int_{\mathcal{B}} \delta  \bm{u}\T \left(\div{\bm{\sigma}} + \rho \bm{b} \right) \dV
```

Dieser Schritt bedeutet, dass wir im Weiteren versuchen werden, die gekoppelten partiellen Differentialgleichungen nicht punktuell exakt, sondern im gewichteten integralen Mittel zu erfüllen. Für elastomechanische Problemstellungen kann die Testfunktion $\delta \bm{u}$ als virtuelle Verrückung aufgefasst werden und die Gleichung {eq}`weakform_02` als das bekannte Prinzip der virtuellen Verrückung aufgefasst werden. Das hier gezeigte Vorgehen ist aber unabhängig von der Elastostatik allgemeingültig.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Als nächstes integrieren wir die Gleichung {eq}`weakform_02` partiell. Dafür wenden wir zunächst die Produktregel auf den Divergenzterm an: 

```{math}
:label: divergenzsatz
\int_{\mathcal{B}} \delta \bm{u}\T \div{\bm{\sigma}} \dV = \int_{\mathcal{B}} \div{\delta \bm{u}\T \bm{\sigma}} \dV -\int_{\mathcal{B}} \underbrace{\grad{\delta\bm{u}\T}}_{\delta \bm{\eb}}\bm{\sigma} \dV \; . 
```

Der Wechsel vom Divergenz-Operator zum Gradienten-Operator im zweiten Term auf der rechten Seite lässt sich schlüssig damit erklären, dass das Resultat ein Skalar sein soll. Während die Divergenz eines Vektors einen Skalar liefert, ergibt der Gradient eines Vektors einen Tensor zweiter Ordnung. Dieser Tensor wird dann in einer doppelten skalaren Multiplikation mit einem weiteren Tensor zweiter Ordnung verknüpft, was letztlich zu einem skalaren Ergebnis führt.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Nun lässt sich mit dem Gauß'schen Ingegralsatz der erste Term auf der rechten Seite von Gleichung {eq}`divergenzsatz` als Integral über den Rand formulieren:

```{math}
:label: gaussIntegralTheorem
\int_{\mathcal{B}} \div{\delta \bm{u}\T  \bm{\sigma}} \dV = \int_{\partial\mathcal{B}} \delta \bm{u}\T \bm{\sigma}  \bm{n} \dA = \int_{\partial\mathcal{B}} \delta \bm{u}\T \bm{t} \dA \; . 
```

Dieses Resultat wird jetzt wieder in Gleichung {eq}`weakform_02` eingesetzt und wir erhalten:

```{math}
:label: weakform_03
\int_{\mathcal{B}} \delta\eb\T  \bm{\sigma} \dV = \int_{\mathcal{B}} \delta \bm{u}\T \rho \bm{b} \dV + \int_{\partial\mathcal{B}} \delta \bm{u}\T  \bm{t} \dA \; .
```
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{admonition} Was haben wir damit erreicht?
:class: tip

- der Oberflächenterm in {eq}`weakform_03` entspricht den Kraftrandbedingungen 
- der Term auf der linken Seite in {eq}`weakform_03` enthält nur noch Ableitungen 1. Ordnung von $\bm{u}$, während in {eq}`weakform_01` Ableitungen 2. Ordnung gefordert wurden $\rightarrow$ schwächere Anforderungen an \bm{u}!
- Gleichung {eq}`weakform_03` ist exakt das Prinzip der virtuellen Verrückungen
  -  virtuelle Arbeit der Spannungen an den Verzerrungen ist gleich der virtuelle Arbeit der eingeprägten Volumenkräfte plus die virtuelle Arbeit der Oberflächenkräfte
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Setzen wir jetzt noch als Materialmodell die Beschreibung der linearen Elastizität ein, so erhalten wir final:

```{math}
:label: weakform_04
\int_{\mathcal{B}} \delta\eb\T \bm{C} \eb \dV = \int_{\mathcal{B}} \delta \bm{u}\T \rho \bm{b} \dV + \int_{\partial\mathcal{B}} \delta \bm{u}\T \cdot \bm{t} \dA \; .
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Die Schwache Form des Stabes

```{figure} images/Stab.png
---
height: 400px
name: stab
---
Stab unter Eigengewicht und externer Kraft.
```
Auch hier beginnen wir mit der starken Form der Differentialgleichung:
```{math}
:label: stabdglsimple
 u^{\prime \prime} =  - \frac{n(x)}{EA} \; ,
```

```{admonition} Analytische Lösung
:class: tip
Die analytische Lösung erhalten wir durch zweifache Integration und einsetzen der Randbedingungen:
  \begin{equation}
  u(x) = \left( \frac{F}{EA} + \frac{n \ell}{E}\right)\cdot x - \frac{1}{2} \frac{n}{E} x^2 \; . 
  \end{equation}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Zur Herleitung der Finiten Elemente Form multiplizieren  wir die starke Form mit der Testfunktion $\delta u$ und integrieren die Gleichung über das Gebiet:
```{math}
:label: stabdglsimple_weak
 \int_{0}^{\ell} \delta u u^{\prime \prime} \dx = \int_{0}^{\ell} - \delta u \frac{n(x)}{EA} \dx\; .
```
Der Ausdruck auf der rechten Seite wird umgeformt zu:
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
EA \int_0^{\ell} (\delta a_1 + 2 \delta a_2 x) (a_1 +2 a_2 x) \dx = F (a_1 \ell +a_2 \ell^2) + \int_0^{\ell} (a_1 x + a_2 x^2) n \dx \; .
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


## Finite Elemente Formulierung für den Stab

```{figure} images/Stab_element.png
---
width: 600px
name: stabelement
---
Stab unter Eigengewicht und externer Kraft mit Stabelementen diskretisiert.
```
Im vorangegangenen Abschnitt haben wir uns von der Effektivität des Prinzips der virtuellen Arbeit in Kombination mit dem Ritz'schen Verfahren überzeugt. Es erweist sich jedoch oft als schwierig, bei komplex geformten Strukturen, die einer vielschichtigen Belastung ausgesetzt sind, einen adäquaten Näherungsansatz zu finden. Die Finite-Elemente-Methode (FEM) löst dieses Problem, indem sie simple Ansatzfunktionen auf überschaubar gestalteten Teilbereichen – den sogenannten finiten Elementen – anwendet und diese dann systematisch für das gesamte Gebiet integriert.

Als Beispiel hierfür dient die im Bild {numref}`stabelement` dargestellte Diskretisierung eines Stabes mit der Gesamtlänge $\ell$, der in zwei finite Elemente mit jeweils der Länge $l_i = \frac{\ell}{2}$ unterteilt wird. 

```{admonition} Allgemeines zu Finiten Elementen
:class: tip

Ein Finites Element besteht in dem vorliegenden Beispiel aus 2 Knoten. Sowohl die Knoten, als auch die Elemente werden im Allgemeinen nummeriert, damit man sie eindeutig ansprechen kann. Im Bild sind die Elementnummern in den rechteckigen Kästen neben dem Element eingetragen. Die Knotennummer sind in den Kreisen neben den Knoten dargestellt. Die Knoten sind die Träger der primären Feldvariablen (hier: Verschiebung $u$) und Ziel der Finiten Elemente Berechnung ist die Bestimmung der primären Variablen, auch Freiheitsgrade (English: Degree of freedom **Dof**) an den Knoten. Im Bild {numref}`stabelement` auf der rechten Seite ist ein einzelnes Stabelement dargestellt. Das Stabelemet hat 2 lokale Knoten und um die Verschiebung an diesen Knoten eindeutig zuzuordnen verwenden wir die Notation:

$
u_1^{e} \text{ Verschiebung u am lokalen Knoten 1 von Element e}
$

$
u_2^{e} \text{ Verschiebung u am lokalen Knoten 2 von Element e}
$

```

Auf diesen Elementen werden dann einfache Ansatzfunktionen verwendet, welche eine lineare Approximation des Verschiebungsfeldes darstellen:

```{math}
:label: LinearDisplacementAnsatz
\begin{align}
 N_1(\xi) & = \xi \qquad N_2(\xi)=(1-\xi) \\
 u_h(\xi) & = N_1(\xi) u_1^{(e)}+ N_2(\xi) u_2^{(e)} \\
 u_h(\xi) & = \sum_{I=1}^2 N_I(\xi) u_I^{(e)}
 \end{align}
```

hierbei wurde das lokale Koordinatensystem $\xi = \frac{x}{\ell_e} \; \in \; [0,1]$  eingeführt, damit der gewählte Ansatz für alle Stabelmenete, unabhängig der Stablänge $\ell_e$ gültig ist. In der Matrixschreibweise erhält man für den Ausdruck {eq}`LinearDisplacementAnsatz`:

```{math}
:label: LinearDisplacementAnsatz2
\begin{align}
  u_h(\xi) & = \underbrace{\begin{bmatrix} \xi & (1-\xi) \end{bmatrix}}_{\bm{N}\T} \underbrace{\begin{bmatrix} u_1^{(e)} \\ u_2^{(e)}\end{bmatrix}}_{\bm{u}^{(e)}} = \bm{N}\T \bm{u}^{(e)}
 \end{align}
```

Zur Berechnung des Prinzips der virtuellen Verrückungen bzw. der schwachen Form wird zusätzlich die 

