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


<!-- ```{admonition} Todo
:class: warning
- [x] Motivation
- [x] Lernziele
- [x] Bilanzgleichungen
   - [x] lokale Form 
   - [x] Randbedingungen
   - [x] Anwendung
   - [x] Zusammenfassende Tabelle
- [x] Kinematik
   - [x] lineare Kinematik
- [x] Materialmodellierung
   - [x] Wärmeleitung nach Fourier
   - [x] Lineare Elastizität
   - [x] Ausblick
   - [x] Ebener Spannungszustand
   - [x] Ebener Verzerrungszustand
   - [x] Rotationssymmetrie
``` -->

```{admonition} Lernziele
:class: important
- Wie werden mechanische Problemstellungen formuliert?
- Was sind die Unbekannten der Bilanzgleichungen?
- Welche Zutaten braucht man um ein lösbares Problem zu erhalten?
- Wie lauten die wesentlichen Bilanzgleichungen der Kontinuumsmechanik?
- In welche Kategorien werden Randbedingungen untergliedert?
- Wozu brauchen wir Materialmodelle?
- Welches sind die grundlegenden Materialmodelle der Wärmeleitung, Fluidtransport und der Strukturmechanik?
- Mit welchen Vereinfachungen kann die Dimensionalität der Problemstellung reduziert werden?
- Was sind die Vorraussetzungen hierfür?
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

# Mathematische Modellbildung in der Kontinuumstheorie

In diesem Kapitel werden die grundlegenden Gleichungen der Kontinuumsmechanik vorgestellt. Dies dient als Ausgangspunkt für die numerische Lösung mechanischer Problemstellungen. Während der Vorstellung der Ausgangsgleichungen wird zudem die verwendete Notation eigeführt.


## Notation

Im Rahmen dieses Moduls wird die folgende symbolische Schreibweise verwendet:


| Tensorstufe                 | Symbol                    | Tensorschreibweise         |
|-----------------------------|---------------------------|----------------------------|
| Tensor der Stufe 0 (Skalar) | $a$, $b$, $\varphi$       | $a$, $b$, $\varphi$        |
| Tensor der Stufe 1 (Vektor) | $\bm{a}$, $\bm{b}$        | $a_i\bm{e}_i$, $b_i\bm{e}_i$ |
| Tensor der stufe 2 (Dyade)  | $\bm{A}$, $\bm{\sigma}$   | $A_{ij}\bm{e}_i\bm{e}_j$, $B_{ij}\bm{e}_i\bm{e}_j$ |    
| Tensor der stufe 4          | $\mathbb{C}$, $\mathbb{A}$| $\mathbb{C}\bm{e}_i\bm{e}_j\bm{e}_k\bm{e}_l$, $\mathbb{A}\bm{e}_i\bm{e}_j\bm{e}_k\bm{e}_l$ |

Soweit möglich erhalten Tensoren der Stufe 2 Großbuchstaben als Symbol, während Vektoren mit Kleinbuchstaben dargestellt werden. Handschriftlich wird ein Tensor durch einen Unterstrich repräsentiert $\underline{A}=\bm{A}$. Für Vektoren ist alternativ auch der Vektorpfeil gebräuchlich $\underline{a} = \vec{a} = \bm{a}$.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Bilanzgleichungen der Kontinuumsmechanik


In diesem Abschnitt werden wir die grundlegenden Bilanzgleichungen der Kontinuumsmechanik einführen. Diese Gleichungen sind das Fundament für die Beschreibung des physikalischen Verhaltens und bilden den Ausgangspunkt für die Finite-Elemente-Methode. Die Bilanzgleichungen repräsentieren Erhaltungsprinzipien, die für jedes Kontinuum gelten, unabhängig von den spezifischen Materialeigenschaften. Die Bilanzgleichungen dienen als Bestimmungsgleichung für die Feldgrößen:
  - Druck $p(\bm{x},t)$
  - Verschiebung $\bm{u}(\bm{x},t)$
  - Temperatur $\theta(\bm{x},t)$.

Als Feldgröße bezeichnet man physikalische Größen, denen zu jedem Zeitpunkt $t$ und zu jedem materiellen Punkt $\bm{x}$ ein Wert zugeordnet wird.

Die vier wesentlichen Bilanzgleichungen sind:

- Massenerhaltung
- Impulserhaltung
- Drehimpulserhaltung (hieraus resultiert die Symmetrie des Spannungstensors $\bm{\sigma}$)
- Energieerhaltung (1. Hauptsatz der Thermodynamik)

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Massenerhaltung

```{figure} images/Massenbilanz.jpg
---
height: 200px
name: massenbilanz
---
Ein- und Ausfluss eines infinitesimalen Volumenelementes **Austauschen**
```

Die Massenerhaltung besagt, dass die Masse eines geschlossenen Systems konstant bleibt, oder wie in der Abbildung, dass die zeitliche Änderung der Masse eines Systems gleich der Differenz von Zustrom und Abfluss ist. In Abwesenheit von Massenquellen oder -senken kann dies mathematisch ausgedrückt werden als:

```{math}
:label: Massenbilanz
\dot{\varrho} +  \div{\varrho {\bm{v}}} = 0
```

Hier ist $\varrho$ die Dichte und $\bm{v}$ die Geschwindigkeit des Materials. Die zeitliche Ableitung einer Größe $\square$ wird durch einen Punkt über der Größe dargestellt $\td{\square}$.

```{note}
Die Massenbilanz ist für gewöhnliche Festkörper stets erfüllt und muss nicht separat gelöst werden. Anders sieht dies im Bereich der Biomechanik oder Geomechanik aus. Hier gibt es z.B. Wachstumsprozesse (Knochen) oder Sickerströmungen. Mit der Massenbilanz kann man die Konzentration $c(\bm{x},t)$ eines Bestandteils oder den Druck $p(\bm{x},t)$ einer Phase bestimmen.
```

Formuliert man die Massenbilanz für Fluide, dann ist die Massendichte eine Funktion des Druckes $p$ und man kann die Zeitableitung in {eq}`Massenbilanz` über die Kettenregel auswerten:

```{math}
:label: Massenbilanz2
 \underbrace{\Pd{\varrho}{p}}_{=:\frac{\varrho}{\kappa}} \td{p} + \div{\varrho {\bm{v}}} = 0 \; .
```

Die primäre Größe nach der die partielle Differentialgleichung gelöst wird ist damit der Druck $p$. Als sekundäre Größe bezeichnet man Feldgrößen, welche von den primären Größen abgeleitet wurden. Im Fall der Massenbilanz des Fluides ist dies die Geschwindigkeit der Materialpartikel $\bm{v}$. Die Gleichungen {eq}`Massenbilanz` und {eq}`Massenbilanz2` beschreiben wie sich die bilanzierte Größe ($p$) in der Zeit verändert, eine Information über den absoluten Wert der Größe erhält man erst mit der Einführung von Anfangs- und Randwerten:

\begin{align}
p(\bm{x},t=t_0) & = \tilde{p}_0 \qquad &\forall \bm{x} \in \mathcal{B}& \\
p(\bm{x},t) & = \tilde{p} \qquad &\forall \bm{x} \in \partial \mathcal{B}_p& \\
\varrho \bm{v}\T \bm{n} & = \tilde{q}_m \qquad &\forall \bm{x} \in \partial\mathcal{B}_m&
\end{align}

Hierbei wird der zweite Term auch als Dirichlet oder wesentliche Randbedingung und der dritte Term als Neumann oder natürliche Randbedingung bezeichnet. 

Diese Art der mathematischen Problemstellung nennt man Anfangsrandwertproblem (IBVP - Initial Boundary Value Problem).

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{admonition} IVBP1 - Massenerhaltung
:class: tip
Bestimme das Druckfeld $p(\bm{x},t): \mathcal{B} \times [t_0,t] \rightarrow \mathcal{R}^1$, sodass für alle materiellen Punkte $\bm{x} \, \in \, \mathcal{B}$ zu jedem Zeitpunt $t \, \in \, [t_0,t]$ die Bilanzgleichung:

$$
\frac{\varrho}{\kappa} \td{p} + \div{\varrho {\bm{v}}} = 0 
$$

unter Einhaltung der Anfangs- und Randbedingungen:

$$
p(\bm{x},t=t_0) & = \tilde{p}_0 \qquad &\forall \bm{x} \in \mathcal{B}& \\
p(\bm{x},t) & = \tilde{p} \qquad &\forall \bm{x} \in \partial \mathcal{B}_p& \\
\varrho \bm{v}\T \bm{n} & = \tilde{q}_m \qquad &\forall \bm{x} \in \partial\mathcal{B}_m&
$$

erfüllt ist.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Impulserhaltung
```{figure} images/Impulsbilanz.jpg
---
height: 200px
name: impulsbilanz
---
Körper $\mathcal{B}$ unter Einwirkung der externen Oberflächenkraft $\tilde{\bm{t}}$ und der Volumenlast $\bm{b}$
```

Die Impulserhaltung, oft formuliert als Newtons zweites Gesetz, besagt, dass die Änderung des Impulses $\rho \bm{v}$ eines Körpers gleich der Summe der auf ihn wirkenden Kräfte ist. Für ein Kontinuum wird dies durch die Cauchy-Bewegungsgleichungen ausgedrückt:

\begin{equation}
 \varrho \td{\bm{v}} =\div{\bm{\sigma}} + \rho \bm{b}
\end{equation}

$\boldsymbol{\sigma}$ steht hierbei für den Spannungstensor und $\mathbf{b}$ für die Volumenkraft pro Masseneinheit.

```{note}
Die Impulsbilanz ist eine vektorielle partielle Differentialgleichung. Mit ihrer Hilfe kann man das Verschiebungsfeld $\bm{u}(\bm{x},t)$ und das Spannungsfeld $\bm{\sigma}(\bm{x},t)$ eines Festkörpers bestimmen.
```

Die primäre Feldgröße dieser Anfangsrandwertproblems ist die Verschiebung $\bm{u}(\bm{x},t)$ der Materialpartikel. Die Sekundäre Größe ist die Spannung $\bm{\sigma}(\bm{x},t)$, welche wiederum von den Dehnungen $\bm{\epsilon}$ abhängig ist. Als Randwerte können entweder Verschiebungen (wesentliche Randbedingung) vorgegeben werden oder Oberflächenspannungen (natürliche Randbedingungen). 

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{admonition} IVBP2 - Impulsbilanz
:class: tip
Bestimme das Verschiebungsfeld $\bm{u}(\bm{x},t): \mathcal{B} \times [t_0,t] \rightarrow \mathcal{R}^3$, sodass für alle materiellen Punkte $\bm{x} \, \in \, \mathcal{B}$ zu jedem Zeitpunt $t \, \in \, [t_0,t]$ die Bilanzgleichung:

$$
\varrho \td{\bm{v}} =\div{\bm{\sigma}} + \rho \bm{b} 
$$

unter Einhaltung der Anfangs- und Randbedingungen:

$$
\bm{u}(\bm{x},t=t_0) & = \tilde{\bm{u}}_0 \qquad &\forall \bm{x} \in \mathcal{B}& \\
\bm{u}(\bm{x},t) & = \tilde{\bm{u}} \qquad &\forall \bm{x} \in \partial \mathcal{B}_u& \\
\bm{\sigma}\T \bm{n} & = \tilde{\bm{t}} \qquad &\forall \bm{x} \in \partial\mathcal{B}_{\sigma}&
$$

erfüllt ist.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Energieerhaltung

Die Energieerhaltung stellt sicher, dass die gesamte Energie in einem abgeschlossenen System erhalten bleibt. Für ein Kontinuum kann die Energiebilanz folgendermaßen ausgedrückt werden:

```{math}
:label: 1HS_01
\rho \MtdFull{e} = \bm{\sigma}\T \td{\bm{\epsilon}} - \div{\bm{q}} + \varrho r
```

Hier ist $e$ die spezifische innere Energie, $\bm{q}$ der Wärmeflussvektor und $r$ die Wärmequelle pro Masseneinheit.

```{note}
Die skalare Energiebilanzgleichung dient zur Berechnung des Temperaturfeldes $\theta(\bm{x},t)$.
```

Vernachlässigt man die Kopplung zwischen Verschiebung und Temperatur erhält man aus Gleichung {eq}`1HS_01` die bekannte instationäre Wärmeleitungsgleichung:

\begin{equation}
\varrho c_e \Pd{\theta}{t} + \div{\bm{q}} - \varrho r = 0
\end{equation}

Hier ist die primäre Variable die Temperatur $\theta(\bm{x},t)$ und die sekundäre Variable der Wärmeflussvektor $\bm{q}(\bm{x},t)$. Gemeinsam mit den Rand- und Anfangsbedingungen erhält man das Anfangsrandwertproblem:

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{admonition} IVBP3 - Energiebilanz
:class: tip
Bestimme das Temperaturfeld $\theta(\bm{x},t): \mathcal{B} \times [t_0,t] \rightarrow \mathcal{R}^1$, sodass für alle materiellen Punkte $\bm{x} \, \in \, \mathcal{B}$ zu jedem Zeitpunt $t \, \in \, [t_0,t]$ die Bilanzgleichung:

$$
\varrho c_e \Pd{\theta}{t} + \div{\bm{q}} - \varrho r = 0
$$

unter Einhaltung der Anfangs- und Randbedingungen:

$$
\theta(\bm{x},t=t_0) & = \tilde{\theta}_0 \qquad &\forall \bm{x} \in \mathcal{B}& \\
\theta(\bm{x},t) & = \tilde{\theta} \qquad &\forall \bm{x} \in \partial \mathcal{B}_{\theta}& \\
\bm{q}\T \bm{n} & = \tilde{\bm{q}} \qquad &\forall \bm{x} \in \partial\mathcal{B}_q&
$$

erfüllt ist.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Zusammenfassung

In den vorangestellten Abschnitten wurde die Bilanzgleichungen der Kontinuumsmechanik verwendet um die für die Ingenieurpraxis wichtigen Anfangsrandwertprobleme der Fluid-, Struktur- und Thermomechanik aufzustellen. Für diese Systeme von gekoppelten partiellen Differentialgleichungen gibt es nur in wenigen Sonderfällen analytische Lösungen. Einige Davon sind uns aus den Vorlesungen zur Elastostatik und Stömungslehr bekannt. Aus diesem Grund haben sich numerische Berechnungsverfahren durchgesetzt. Um zu überprüfen ob die Anfangsrandwertprobleme in der obigen Form lösbar sind werden in der nachfolgenden Tabelle die Bestimmungsgleichungen und die Unbekannten gegenübergestellt.

| Bestimmungsgleichung | Anzahl an Gleichungen | Unbekannte   | Anzahl an Unbekannten | Anzahl an zusätzlichen Bedingungen |
|----------------------|-----------------------|--------------|-----------------------|------------------------------------|
| Massenbilanz         | 1                     | $p$, $\bm{v}$| 4                     | 3                                  |
| Impulsbilanz         | 3                     | $\bm{u}$, $\bm{\sigma}$, $\bm{\epsilon}$| 15 | 12                         |
| Energiebilanz        | 1                     | $\theta$, $\bm{q}$| 4                | 3                                  |

Wie leicht zu erkennen ist genügt die Anzahl an Gleichungen für keines der obigen Problemstellungen zur Lösung. Es werden darum zusätzliche Bedingungen formuliert um die Anfangsrandwertprobleme zu lösen:

- Kinematische Bedingungen
- Materialmodelle

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Kinematik

Der Kinemtischen Bedingungen haben besonders im Bereich der Strukturmechanik und der Lösung der Impulsbilanz eine hervorgehobene Bedeutung. So ist zum Beispiel die Forderung vom ebenbleiben des Querschnitts in der Bernoulli Balkentheorie eine starke kinematische Restriktion, welche auf die bekannte Differentialgleichung der Durchbiegung $w(x)$ führt. 
Im Rahmen der linearen Kontinuumsmechanik wird die Dehnung $\bm{\epsilon}$ als symmetrischer Anteil des Verschiebungsgradienten $\grad{\bm{u}}$ verstanden:

```{math}
:label: linearStrain
\bm{\epsilon} = \frac{1}{2} \left(\grad{\bm{u}}\T + \grad{\bm{u}} \right) \; .
```

Dieses Dehnungsmaß wird oft auch als Ingenieurdehnung bezeichnet und sollte nur in einem Dehnungsbereich < 10% angewendet werden. Zudem ist hervorzuheben, dass Starrkörpertranslationen keine Ingenieurdehnung hervorrufen, Starrkörperrotationen hingegen sehrwohl. Aus diesem Grunde ist darauf zu achten, dass beim Auftreten größerer Rotationen stehts eine nichtlineare Theorie (2.Ordnung oder allgemein nichtlinear) verwendet wird.

Aufgrund der Symmetrie des Dehnungstensors erhalten wir **6** zusätzliche Bedingungen zur Lösung der Anfangsrandwertproblems der Impulsbilanz.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Materialmodell

Was zur Schließung der mathematischen Problemstellung jetzt noch fehlt ist eine Relation zwischen der primären Feldgröße und ihrer zugeordneten Flussgröße (sekundäre Größe). Die oben beschriebenen Bilanzgleichungen sind physikalische Prinzipien die unabhängig vom Werkstoff Gültigkeit besitzen. Die spezifischen Werkstoffeigenschaften werden über Materialmodelle, sogenannte konstitutive Modelle, in das Anfangsrandwertproblem eingebracht. Die konstitutive Materialtheorie ist eine Wissenschaft für sich und soll in dieser Vorlesung nicht weiter behandelt werden. In der Strukturmechanik können Materialien entsprechend ihrer Eigenschaften eingeteilt werden in:

```{figure} ./images/Materialklassen.png
---
height: 500px
name: materialklassen
---
Klassifizierung von Materialmodellen anhand ihrer Systemantwort. Abbildung nach {cite}`haupt2013continuum`
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Wärmeleitung

Bei der Berechnung der Wärmeleitungsgleichung wird oftmals **Fourier**sche Wärmeleitung zugrunde gelegt. Diese basiert auf der Annahme, dass der Wärmestrom proportional zum negativen Temperaturgradienten ist:

```{math}
:label: fourier
\bm{q} = - \bm{k}\T  \grad{\theta} \, .
```
Hierbei ist $\bm{k}$ die Konduktivitätstensor, welche für isotrope Materialien wie folgt aussieht:
```{math}
:label: konduktivitaetsmatrixIsotrop
 \bm{k} = k \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1\end{pmatrix}  \, ,
```
mit der Wärmeleitfähigkeit $k$ in $\frac{\text{W}}{\text{m}\cdot\text{K}}$. Der Wärmestromvektor hat damit die Einheit $\frac{\text{W}}{\text{m}^2}$.

```{admonition} Zusätzliche Gleichungen
:class: warning

Aus Gleichung {eq}`fourier` erhalten wir **3** zusätzliche Gleichungen. Wir haben jetzt genausoviele Gleichungen wie Unbekannte. 

```

```{admonition} Materialsymmetrie (Auswahl)
:class: note
- isotrop - gleiche Eigenschaften in alle Raumrichtungen
- transversal isotrop - Unterschiedliche Eigenschaften entlang einer Faser (Symmetrieachse) und der zu dieser Faser senkrecht stehenden Ebene
- orthotrop - drei paarweise senkrecht zueinander stehende Symmetrieachsen
```
 

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Durchströmung

Bei der Berechnung der Fluidgeschwindigkeit in Sickerströmungen wird oftmals das **Darcy**sche hydraulische Strömungsgesetz angewendet. Dies ist analog zur Wärmeleitung definiert als proportional zum negativer Druckgradienten:

```{math}
:label: darcy
\bm{v} = - \bm{k}_{\varepsilon}\T  \grad{p} \, .
```
Hierbei ist $\bm{k}_{\varepsilon}$ die hydraulische Konduktivität. Für isotrope Materialien gilt:

```{math}
 \bm{k}_{\varepsilon} = \frac{k_{\varepsilon}}{\mu} \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1\end{pmatrix}  \, ,
```
mit der hydraulischen Permeabilität $k_{\varepsilon}$ in $\text{m}^2$ und der dynamischen Viskosität $\mu$ in $\text{Pa}\cdot \text{s}$. 

```{admonition} Zusätzliche Gleichungen
:class: warning

Aus Gleichung {eq}`darcy` erhalten wir **3** zusätzliche Gleichungen. Wir haben jetzt genausoviele Gleichungen wie Unbekannte. 

```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Lineare Elastizität

In {numref}`materialklassen` sind verschiedene Klassen von Festkörpermaterialien dargestellt. In dieser Vorlesung behandeln wir lediglich eine Unterklasse der ratenunabhängigen Materialien ohne Hystere. Die Lineare Elastizität ist gekennzeichnet durch einen linearen Zusammenhang zwischen Spannung $\bm{\sigma}$ und Dehnungen $\bm{\epsilon}$. In tensorieller Notation stellt sich das verallgemeinerte Hook'sche Gesetz wie folgt dar:

```{math}
:label: generalHook
\sigma_{ij} = \mathbb{C}_{ijkl} \epsilon_{kl} \;
```

wobei von der Einsteinschen Summenkonvention gebrauch gemacht wurde. In der Strukturmechanik ist es jedoch unüblich mit Tensoren 4. Stufe explizit zu rechnen. Es werden Symmetrieeigenschaften der Tensoren $\sigma_{ij}=\sigma_{ji}$, $\epsilon_{ij}=\epsilon_{ji}$ und $\mathbb{C}_{ijkl}=\mathbb{C}_{klij}$ ausgenutzt um Tensorprodukte wie z.B. in Gleichung {eq}`generalHook` in Matrix-Vektor-Operationen zu überführen. Dies ist vor allem für die numerische Implementierung von großem Vorteil.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{math}
:label: generalHook2
\begin{pmatrix} 
\sigma_{11} \\
\sigma_{22} \\
\sigma_{33} \\
\sigma_{12} \\
\sigma_{23} \\
\sigma_{13} 
\end{pmatrix} = \begin{pmatrix}
C_{1111} & C_{1122} & C_{1133} & C_{1112} & C_{1123} & C_{1113} \\
C_{2211} & C_{2222} & C_{2233} & C_{2212} & C_{2223} & C_{2213} \\
C_{3311} & C_{3322} & C_{3333} & C_{3312} & C_{3323} & C_{3313} \\
C_{1211} & C_{1222} & C_{1233} & C_{1212} & C_{1223} & C_{1213} \\
C_{2311} & C_{2322} & C_{2333} & C_{2312} & C_{2323} & C_{2313} \\
C_{1311} & C_{1322} & C_{1333} & C_{1312} & C_{1323} & C_{1313} \\ 
\end{pmatrix} \begin{pmatrix} \epsilon_{11} \\ \epsilon_{22} \\ \epsilon_{33} \\ 2\epsilon_{12} \\ 2\epsilon_{23} \\ 2\epsilon_{13} \end{pmatrix}
```

Für isotrope lineare Elastizität mit dem Materialkonstanten:
- $E$ - Elastizitätsmodul in $\text{MPa}$
- $\nu$ - Querkontraktionszahl in [-]

erhält man dann:


```{math}
:label: generalHook3
\begin{pmatrix} 
\sigma_{11} \\
\sigma_{22} \\
\sigma_{33} \\
\sigma_{12} \\
\sigma_{23} \\
\sigma_{13} 
\end{pmatrix} = \frac{E}{(1+\nu)(1-2\nu)}\begin{pmatrix}
1-\nu & \nu & \nu & 0 & 0 & 0 \\
\nu & 1-\nu & \nu & 0 & 0 & 0 \\
\nu & \nu & 1-\nu & 0 & 0 & 0 \\
0 & 0 & 0 & \frac{1-2\nu}{2} & 0 & 0 \\
0 & 0 & 0 & 0 & \frac{1-2\nu}{2} & 0 \\
0 & 0 & 0 & 0 & 0 & \frac{1-2\nu}{2} \\ 
\end{pmatrix} \begin{pmatrix} \epsilon_{11} \\ \epsilon_{22} \\ \epsilon_{33} \\ 2\epsilon_{12} \\ 2\epsilon_{23} \\ 2\epsilon_{13} \end{pmatrix}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Für die inverse Beziehung $\bm{\epsilon} = \bm{C}^{-1} \bm{\sigma}$ mit der Nachgiebiegkeitsmatrix $\bm{C}^{-1}$ erhält man:
```{math}
:label: generalHook4
\begin{pmatrix} 
\epsilon_{11} \\
\epsilon_{22} \\
\epsilon_{33} \\
2\epsilon_{12} \\
2\epsilon_{23} \\
2\epsilon_{13} 
\end{pmatrix} =\begin{pmatrix}
\frac{1}{E} & \frac{-\nu}{E} & \frac{-\nu}{E} & 0 & 0 & 0 \\
\frac{-\nu}{E} & \frac{1}{E} & \frac{-\nu}{E} & 0 & 0 & 0 \\
\frac{-\nu}{E} & \frac{-\nu}{E} & \frac{1}{E} & 0 & 0 & 0 \\
0 & 0 & 0 & \frac{1}{G} & 0 & 0 \\
0 & 0 & 0 & 0 & \frac{1}{G} & 0 \\
0 & 0 & 0 & 0 & 0 & \frac{1}{G} \\ 
\end{pmatrix} \begin{pmatrix} \sigma_{11} \\ \sigma_{22} \\ \sigma_{33} \\ \sigma_{12} \\ \sigma_{23} \\ \sigma_{13} \end{pmatrix}
```

wobei $G=\frac{E}{2(1+\nu)}$ den Schubmodul in $\text{MPa}$ darstellt.

```{admonition} Zusätzliche Gleichungen
:class: warning

Aus Gleichung {eq}`generalHook3` und {eq}`generalHook4` erhalten wir **6** zusätzliche Gleichungen. Wir haben jetzt genausoviele Gleichungen wie Unbekannte. 

```

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

```{admonition} Einsteinsche Summenkonvention
:class: note
Die Einsteinsche Summenkonvention ist eine Notationsvereinbarung, die in der Tensorrechnung verwendet wird, um Ausdrücke zu vereinfachen. Wenn ein Index in einem mathematischen Ausdruck zweimal vorkommt, wird implizit über diesen Index summiert:

$$
c_i = \sum_{j=1}^{n} a_{ij} b_j \qquad \rightarrow \qquad  c_i=a_{ij} b_j
$$

```

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

```{admonition} Kelvin Notation vs. Voigt Notation
:class: note

Die Kelvin Notation und die Voigt Notation sind zwei verschiedene Methoden um tensorielle Größen in Matrix- und Vektorschreibweise zu überführen. In den meisten kommerziellen FEM Programmen ist die Voigt Notation vorzufinden, wobei neuere FE-Codes durchaus die Vorteile der Kelvin Notation ausnutzen {cite}`nagel2016advantages`. Allgemein kann gesagt werden, dass die Voigt Notation näher an der Ingenieurmechanik ist, da die Scheranteile $\epsilon_{ij} \quad \forall i\neq j$ der Dehnung doppelt eingehen und wir somit die Gleitungen $\gamma_{ij}\quad \forall i\neq j$ erhalten. Die Kelvin Notation hat den Vorteil, dass mit ihr weiterhin alle Tensorprodukte einfach gebildet werden können ohne das über den Vorfaktor der Komponenten nachgedacht werden muss. Im aktuellen Kurs wird die **Voigt**-Notation zugrunde gelegt. 


|                    |   Voigt Notation    |
|--------------------|---------------------|
|$\sigma_{ij}$       | $\bm{\sigma}= \begin{pmatrix} \sigma_{11} & \sigma_{22} & \sigma_{33} & \sigma_{12} & \sigma_{23} & \sigma_{13} \end{pmatrix}\T $ |
|$\epsilon_{ij}$       | $\bm{\epsilon}= \begin{pmatrix} \epsilon_{11} & \epsilon_{22} & \epsilon_{33} & 2\epsilon_{12} & 2\epsilon_{23} & 2\epsilon_{13} \end{pmatrix}\T $ |
| $\sigma_{ij}=\mathbb{C}_{ijkl}\epsilon_{kl}$| $\bm{\sigma} = \bm{C} \bm{\epsilon} $|
| $\mathcal{E}=\sigma_{ij}\epsilon_{ij}$| $ \mathcal{E} = \bm{\sigma}\T\bm{\epsilon}$|

$$
\mathbb{C}_{ijkl} \quad \rightarrow \quad \bm{C}=\begin{pmatrix}
C_{1111} & C_{1122} & C_{1133} & C_{1112} & C_{1123} & C_{1113} \\
C_{2211} & C_{2222} & C_{2233} & C_{2212} & C_{2223} & C_{2213} \\
C_{3311} & C_{3322} & C_{3333} & C_{3312} & C_{3323} & C_{3313} \\
C_{1211} & C_{1222} & C_{1233} & C_{1212} & C_{1223} & C_{1213} \\
C_{2311} & C_{2322} & C_{2333} & C_{2312} & C_{2323} & C_{2313} \\
C_{1311} & C_{1322} & C_{1333} & C_{1312} & C_{1323} & C_{1313} \\ 
\end{pmatrix} 
$$

|                    |    Kelvin Notation  |
|--------------------|---------------------|
|$\sigma_{ij}$        | $\bm{\sigma}= \begin{pmatrix} \sigma_{11} & \sigma_{22} & \sigma_{33} & \sqrt{2}\sigma_{12} & \sqrt{2}\sigma_{23} & \sqrt{2}\sigma_{13} \end{pmatrix}\T $ |
|$\epsilon_{ij}$        | $\bm{\epsilon}= \begin{pmatrix} \epsilon_{11} & \epsilon_{22} & \epsilon_{33} & \sqrt{2}\epsilon_{12} & \sqrt{2}\epsilon_{23} & \sqrt{2}\epsilon_{13} \end{pmatrix}\T $ |
| $\sigma_{ij}=\mathbb{C}_{ijkl}\epsilon_{kl}$| $\bm{\sigma} = \bm{C} \bm{\epsilon}$ |
| $\mathcal{E}=\sigma_{ij}\epsilon_{ij}$| $ \mathcal{E} = \bm{\sigma}\T\bm{\epsilon}$|

$$
\mathbb{C}_{ijkl} \quad \rightarrow \quad \bm{C}=\begin{pmatrix}
C_{1111} & C_{1122} & C_{1133} & \sqrt{2}C_{1112} & \sqrt{2}C_{1123} & \sqrt{2}C_{1113} \\
C_{2211} & C_{2222} & C_{2233} & \sqrt{2}C_{2212} & \sqrt{2}C_{2223} & \sqrt{2}C_{2213} \\
C_{3311} & C_{3322} & C_{3333} & \sqrt{2}C_{3312} & \sqrt{2}C_{3323} & \sqrt{2}C_{3313} \\
\sqrt{2}C_{1211} & \sqrt{2}C_{1222} & \sqrt{2}C_{1233} & 2C_{1212} & 2C_{1223} & 2C_{1213} \\
\sqrt{2}C_{2311} & \sqrt{2}C_{2322} & \sqrt{2}C_{2333} & 2C_{2312} & 2C_{2323} & 2C_{2313} \\
\sqrt{2}C_{1311} & \sqrt{2}C_{1322} & \sqrt{2}C_{1333} & 2C_{1312} & 2C_{1323} & 2C_{1313} \\ 
\end{pmatrix} 
$$

```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

#### Ebener Verzerrungszustand

```{figure} ./images/EVZ.png
---
height: 300px
name: evz
---
Beispiel für das Auftreten eines ebenen Verzerrungszustandes {cite}`chaves2013notes`
```
Betrachten wir ein Strukturelement mit prismatischen Merkmalen, bei dem die Dimension in Richtung der prismatischen Achse deutlich größer ist als die anderen Dimensionen. Die aufgebrachten Lasten wirken senkrecht zur prismatischen Achse ({numref}`evz`). Unter diesen Bedingungen sind die Dehnungskomponenten $\e_{13},\, \e_{23}, \, \e_{33}$ gleich null. Dieser Zustand wird als ebener Verzerrungszustand bezeichnet. Beispiele hierfür sind Stützmauern, unter Druck stehende Zylinder, Dämme , Tunnel und Flachgründungen.

Es muss betont werden, dass die Variablen (Last, Querschnitt, Material) entlang der prismatischen Achse konstant sein müssen, um einen ebenen Verzerrungszustand zu betrachten. Andernfalls können erhebliche Fehler auftreten.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{admonition} Prismatische Merkmale
:class: note
- Konstanter Querschnitt entlang der Längsachse
- Längliche Form 
- Geradlinige Achse

**Beispiele:** Balken, Säulen, Rohre
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Um die konstitutive Beziehung $\sb(\eb)$ zu erhalten starten wir vom generalisierten Hook'schen Gesetz {eq}`generalHook3` indem wir alle Spalten mit korrespondierenden 0-Dehnungen eliminieren:

```{math}
:label: evz1
\begin{pmatrix} 
\sigma_{11} \\
\sigma_{22} \\
\sigma_{33} \\
\sigma_{12} \\
\sigma_{23} \\
\sigma_{13} 
\end{pmatrix} = \frac{E}{(1+\nu)(1-2\nu)}\begin{pmatrix}
1-\nu & \nu & \cancel{\nu} & 0 & \cancel{0} & \cancel{0} \\
\nu & 1-\nu & \cancel{\nu} & 0 & \cancel{0} & \cancel{0} \\
\nu & \nu & \cancel{1-\nu} & 0 & \cancel{0} & \cancel{0} \\
0 & 0 & \cancel{0} & \frac{1-2\nu}{2} & \cancel{0} & \cancel{0} \\
0 & 0 & \cancel{0} & 0 & \cancel{\frac{1-2\nu}{2}} & \cancel{0} \\
0 & 0 & \cancel{0} & 0 & \cancel{0} & \cancel{\frac{1-2\nu}{2}} \\ 
\end{pmatrix} \begin{pmatrix} \epsilon_{11} \\ \epsilon_{22} \\ \cancel{\epsilon_{33}} \\ 2\epsilon_{12} \\ \cancel{2\epsilon_{23}} \\ \cancel{2\epsilon_{13}} \end{pmatrix}
```

Wir sehen sofort, dass die Komponenten $\s_{23},\, \s_{13}$ verschwinden. Die Spannungskomponente entlang der prismatischen Achse $\s_{33}$ hingegen berechnet sich zu:

```{math}
\s_{33} = \frac{E \nu}{(1+\nu)(1-2\nu)} \left(\e_{11} + \e_{22} \right)
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Arrangiert man die verbleibenden Einträge, so erhält man für $\bm{C}\rs{EVZ}$:
```{math}
:label: evz2
 \begin{pmatrix} 
\s_{11} \\
\s_{22} \\
\s_{12} 
\end{pmatrix}= \underbrace{\frac{E }{(1+\nu)(1-2\nu)} \begin{pmatrix}
1-\nu & \nu & 0 \\
\nu & 1-\nu & 0 \\
0 & 0 & \frac{1-2\nu}{2}
\end{pmatrix}}_{\bm{C}\rs{EVZ}}  \begin{pmatrix} 
\e_{11} \\
\e_{22} \\
\e_{12} 
\end{pmatrix}
```

Für die Nachgiebiegkeit $\bm{C}\rs{EVZ}^{-1}$ erhält man die inverse Beziehung:
```{math}
:label: evz3
 \begin{pmatrix} 
\e_{11} \\
\e_{22} \\
\e_{12} 
\end{pmatrix}= \underbrace{\frac{1+\nu }{E} \begin{pmatrix}
1-\nu & -\nu & 0 \\
-\nu & 1-\nu & 0 \\
0 & 0 & 2
\end{pmatrix}}_{\bm{C}\rs{EVZ}^{-1}}  \begin{pmatrix} 
\s_{11} \\
\s_{22} \\
\s_{12} 
\end{pmatrix}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

#### Ebener Spannungszustand

```{figure} ./images/CarCrash.jpg
---
height: 500px
name: carcrash
---
FEM Crash Simulation eines Pickup-Trucks gegen eine starre Wand [Quelle](https://enteknograte.com/wp-content/uploads/2022/06/truck-crash-Car-vehicle-Finite-Element-Simulation-Crash-Test-MSC-dytran-Crashworthiness-Ls-Dyna-Abaqus-PAM-CRASH.jpg)
```

Der ebene Spannungszustand tritt überall dort auf wo Flächenelemente vor allem in ihrer Ebene belastet werden und die Flächenabmessungen deutlich größer sind als die dazugehörige Dicke. Dies ist zum Beispiel bei Karosserieblechen wie in {numref}`carcrash` der Fall. 

```{figure} ./images/Shellformulation.png
---
height: 300px
name: shellformulation
---
FEM Diskretisierung von Schalenelementen als degenerierte Volumenelemente (links) oder über eine Mittelflächendarstellung (rechts). {cite}`wriggers2008nonlinear`
```

In Abbildung {numref}`shellformulation` auf der rechten Seite ist eine Disketisierung für den ebenen Spannungszustand mit Mittelflächenelementen zu sehen.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Startpunkt für die konstitutive Beziehung ist die Gleichung {eq}`generalHook4`. Zunächst streichen wir alle Spalten welche den 0-Spannungen zugeordnet werden können:
```{math}
:label: generalHookESZ1
\begin{pmatrix} 
\epsilon_{11} \\
\epsilon_{22} \\
\epsilon_{33} \\
2\epsilon_{12} \\
2\epsilon_{23} \\
2\epsilon_{13} 
\end{pmatrix} = \begin{pmatrix}
\frac{1}{E} & \frac{-\nu}{E} & \cancel{\frac{-\nu}{E}} & 0 & \cancel{0} & \cancel{0} \\
\frac{-\nu}{E} & \frac{1}{E} & \cancel{\frac{-\nu}{E}} & 0 & \cancel{0} & \cancel{0} \\
{\frac{-\nu}{E}} & {\frac{-\nu}{E}} & \cancel{\frac{1}{E}} & {0} & \cancel{0} & \cancel{0} \\
0 & 0 & \cancel{0} & \frac{1}{G} & 0 & 0 \\
{0} & {0} & \cancel{0} & {0} & \cancel{\frac{1}{G}} & \cancel{0} \\
{0} & {0} & \cancel{0} & {0} & \cancel{0} & \cancel{\frac{1}{G}} \\ 
\end{pmatrix} \begin{pmatrix} \sigma_{11} \\ \sigma_{22} \\ \cancel{\sigma_{33}} \\ \sigma_{12} \\ \cancel{\sigma_{23}} \\ \cancel{\sigma_{13}} \end{pmatrix}
```

Hier fällt auf, dass das resultierende System nicht mehr quadratisch ist, da die Dehnung in Dickenrichtung $\epsilon_{33}$ ungleich null ist:

```{math}
:label: generalHookESZ2
\begin{pmatrix} 
\epsilon_{11} \\
\epsilon_{22} \\
\epsilon_{33} \\
2\epsilon_{12} \\
\end{pmatrix} = \begin{pmatrix}
\frac{1}{E} & \frac{-\nu}{E}  & 0  \\
\frac{-\nu}{E} & \frac{1}{E}  & 0  \\
{\frac{-\nu}{E}} & {\frac{-\nu}{E}} & {0} \\
0 & 0 & \frac{1}{G}  \\
\end{pmatrix} \begin{pmatrix} \sigma_{11} \\ \sigma_{22} \\ \sigma_{12} \end{pmatrix} \; .
```

Dieses System hat eine Gleichung zu viel. Zweckmäßig vernachlässigt man die Gleichung für die Dehnung in Dickenrichtung (diese Dehnung berechnet man in einer Nachlaufrechnung). Somit lautet die Nachgiebigkeitsmatrix $\bm{C}^{-1}\rs{ESZ}$ und die Materielle Steifigkeitsmatrix $\bm{C}\rs{ESZ}$ für den ebenen Spannungszustand:

```{math}
:label: generalHookESZ3
\bm{C}^{-1} = \frac{1}{E} \begin{pmatrix}
1 & -\nu & 0  \\
-\nu & 1  & 0  \\
0 & 0 & 2(1+\nu)  \\
\end{pmatrix}  \qquad 
\bm{C} = \frac{E}{(1-\nu^2)}\begin{pmatrix}
1 & \nu  & 0  \\
\nu & 1  & 0  \\
0 & 0 & \frac{1-\nu}{2}  \\
\end{pmatrix}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

#### Rotationssymmetrie

```{figure} ./images/Rotsym.png
---
height: 300px
name: rotsym
---
Rotationssymmetrischer Körper im zylinderkoordinatensystem $\{r,z,\theta\}$. Nach {cite}`chaves2013notes`
```

Die Dehnungen für rotationssymmetrische Problemstellungen werden berechnet zu:

```{math}
:label: rotsymstrain
\begin{pmatrix}
\e_{r} \\
\e_z \\
\e_{\theta} \\
\gamma_{rz}
\end{pmatrix} = \begin{pmatrix}
\Pd{u}{r} \\
\Pd{v}{z} \\
\frac{u}{r} \\
\Pd{u}{z} + \Pd{v}{r}
\end{pmatrix} 
```

Das generalisierte Hook'sche Gesetz lautet:

```{math}
:label: generalHookRotsym
\begin{pmatrix} 
\sigma_{rr} \\
\sigma_{zz} \\
\sigma_{\theta} \\
\sigma_{rz} \\
\end{pmatrix} = \frac{E}{(1+\nu)(1-2\nu)}\begin{pmatrix}
1-\nu & \nu & \nu & 0  \\
\nu & 1-\nu & \nu & 0  \\
\nu & \nu & 1-\nu & 0  \\
0 & 0 & 0 & \frac{1-2\nu}{2} \\
\end{pmatrix} \begin{pmatrix} \epsilon_{r} \\ \epsilon_{\theta} \\ \epsilon_{z} \\ 2\epsilon_{rz}  \end{pmatrix}
```

```{admonition} Berechnung Dehung $\e_{\theta}$
:class: note

Die Dehnung in Umfangsrichtung kann bestimmt werden als Längenänderung des Umfangs infolge einer radialen Verschiebung $u$:

$
\e_{\theta} = \frac{2 \pi (r+u) -2\pi r}{2\pi r} = \frac{u}{r}
$

```

```{code-cell} ipython3

```
