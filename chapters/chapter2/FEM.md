---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.7
---

+++ {"editable": true, "slideshow": {"slide_type": "skip"}}


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


# Von der starken zur schwachen Form

Für die in Kapitel 1 eingeführten Randwertprobleme (Massenbilanz, Impulsbilanz und Energiebilanz) wird in diesem Kapitel die theoretischen Grundlagen der Finite Elemente Methode beschrieben. Für jedes der genannten Probleme ist der Ausgangspunkte die **starke** Form der beschreibenden partiellen Differentialgleichung. Am Beispiel der Impulsbilanz soll exemplarisch die zugehörige **schwache** Form hergeleitet werden. Die Begrifflichkeit stark und schwach bezieht sich hierbei auf die Stetigkeits- und Differenzierbarkeitsanforderungen der gesuchten Lösung $\bm{u}$. Die starke Form hat somit **immer** höhere Anforderungen an die Lösung.

```{figure} ../chapter1/images/Impulsbilanz.jpg
---
height: 400px
name: impulsbilanz_fig
---
Körper $\mathcal{B}$ unter Einwirkung der externen Oberflächenkraft $\tilde{\bm{t}}$ und der Volumenlast $\bm{b}$
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Ausgangspunkt für die exemplarische Herleitung ist die Impulsbilanz in der quasistatischen Form (ohne Trägheitsterme):

```{math}
:label: weakform_01
0 = \div{\bm{\sigma}} + \rho \bm{b} 
```
Diese starke Form wird jetzt mit einer beliebigen vektorwertigen Testfunktion $\delta \bm{u}$ multipliziert und über das betrachtete Gebiet $\mathcal{B}$ (den betrachteten Körper) integriert.

```{math}
:label: weakform_02
0 = \int_{\mathcal{B}} \delta  \bm{u}\T \left(\div{\bm{\sigma}} + \rho \bm{b} \right) \dV
```

Dieser Schritt bedeutet, dass wir im Weiteren versuchen werden, die gekoppelten partiellen Differentialgleichungen nicht punktuell exakt zu erfüllen, sondern im gewichteten integralen Mittel. Für elastomechanische Problemstellungen kann die Testfunktion $\delta \bm{u}$ als virtuelle Verrückung aufgefasst werden, und die Gleichung {eq}`weakform_02` als das bekannte Prinzip der virtuellen Verrückung angesehen werden. Das hier gezeigte Vorgehen ist jedoch unabhängig von der Elastostatik allgemeingültig.

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
- der Term auf der linken Seite in {eq}`weakform_03` enthält nur noch Ableitungen 1. Ordnung von $\bm{u}$, während in {eq}`weakform_01` Ableitungen 2. Ordnung gefordert wurden $\rightarrow$ schwächere Anforderungen an $\bm{u}$!
- Gleichung {eq}`weakform_03` ist exakt das Prinzip der virtuellen Verrückungen
  -  virtuelle Arbeit der Spannungen an den Verzerrungen ist gleich der virtuelle Arbeit der eingeprägten Volumenkräfte plus die virtuelle Arbeit der Oberflächenkräfte
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


Setzen wir jetzt noch als Materialmodell die Beschreibung der linearen Elastizität ein, so erhalten wir final:

```{math}
:label: weakform_04
\int_{\mathcal{B}} \delta\eb\T \bm{C} \eb \dV = \int_{\mathcal{B}} \delta \bm{u}\T \rho \bm{b} \dV + \int_{\partial\mathcal{B}} \delta \bm{u}\T \cdot \bm{t} \dA \; .
```


```{admonition} Fragen zum Kapitel
:class: warning

**Finite Elemente Methode**
- Worauf beziehen sich die Begriffe *starke* und *schwache* Formulierung?
- Was ist korrekt:
  - [ ] Bei der schwachen Formulierung wird die Bilanzgleichung nur noch im integralen Mittel gelöst.
  - [ ] Das Prinzip der virtuellen Verrückung und die schwache Formulierung der Impulsbilanz sind äquivalent.
  - [ ] Die starke Formulierung der Bilanzgleichung enthält Ableitungen höherer Ordnung im Vergleich zur schwachen Formulierung. 

```