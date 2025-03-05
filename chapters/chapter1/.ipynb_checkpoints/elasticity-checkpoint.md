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

$$
\newcommand{\tbf}{\textbf}% text bold
\newcommand{\tit}{\textit}% text italic
\newcommand{\tsl}{\textsl}% text slanted
\newcommand{\tsc}{\textsc}% text small cap
\newcommand{\ttt}{\texttt}% text typewriter
\newcommand{\trm}{\textrm}% text roman
\newcommand{\tsf}{\textsf}% text sans serif
\newcommand{\tup}{\textup}% text upright
\newcommand{\mbf}{\mathbf}% math bold
\providecommand{\mit}{\mathit}% math italic
\newcommand{\msf}{\mathsf}% math sans serif
\newcommand{\mrm}{\mathrm}% math roman
\newcommand{\mtt}{\mathtt}% math typewriter
\newcommand{\dcdot}{\mathbf{:}}
\newcommand{\bm}[1]{\boldsymbol{#1}}
\newcommand{\checkanddefine}[2]{
   \ifundef{#1}{\newcommand{#1}{#2}}{\renewcommand{#1}{#2}}
}
\ifundef{\tr}{
\newcommand{\tr}[1]{\text{tr}\left(#1\right)}
}{
   \renewcommand{\tr}[1]{\text{tr}\left(#1\right)}
}
\newcommand{\Grad}{\trm{Grad}\,}
\renewcommand{\Grad}[1]{\trm{Grad}\left(#1\right)}
\newcommand{\grad}{\mathrm{grad}\,}
\renewcommand{\grad}[1]{\trm{grad}\left(#1\right)}
\newcommand{\Div}{\trm{Div}\,}
\renewcommand{\Div}[1]{\trm{Div} \left(#1\right)}
\renewcommand{\div}{\trm{div}\,}
\renewcommand{\div}[1]{\trm{div} \left(#1\right)}
\newcommand{\Lin}{\textrm{Lin}\,}
\newcommand{\intd}{\,{\rom{d}}}
\newcommand{\la}{\, \leftarrow \,}
\newcommand{\dV}{\; \textrm{d}V}
\newcommand{\dViso}{\; \textrm{d}V_{\square}}
\newcommand{\dA}{\; \textrm{d}A}
\newcommand{\da}{\; \textrm{d}a}
\newcommand{\dx}{\; \textrm{ d}x}
\newcommand{\dy}{\; \textrm{ d}y}
\newcommand{\dz}{\; \textrm{ d}z}
\newcommand{\dX}{\; \textrm{ d}X}
\newcommand{\dY}{\; \textrm{ d}Y}
\newcommand{\dZ}{\; \textrm{ d}Z}
\newcommand{\dxi}{\; \textrm{ d}\xi}
\newcommand{\deta}{\; \textrm{ d}\eta}
\newcommand{\dzeta}{\; \textrm{ d}\zeta}
\newcommand{\Ibt}{\int_{\mathcal{B}_t}}
\newcommand{\Ist}{\int_{\Gamma_t}}
\newcommand{\Ibref}{\int_{\mathcal{B}\rs{0}}}
\newcommand{\Isref}{\int_{\Gamma\rs{0}}}
\newcommand{\sref}{{\Gamma\rs{0}}}
\newcommand{\Bref}{{\mathcal{B}\rs{0}}}
\newcommand{\Ibiso}{\int_{\mathcal{B}_{\square}}}
\newcommand{\siso}{{\Gamma_{\square}}}
\newcommand{\Mtd}[1]{\left(#1\right)^{,}}
\newcommand{\MtdFull}[1]{\frac{D \, #1}{D \, t}}
\newcommand{\Pd}[2]{\frac{\partial #1}{\partial #2}}
\newcommand{\dt}[1]{#1 ^{\bullet}}
\newcommand{\rom}[1]{\textrm{#1} }
\newcommand{\rp}[1]{^{\rom{#1}}}
\newcommand{\rs}[1]{_{\rom{#1}}}
\newcommand{\T}{\rp{T}}
\newcommand{\mT}{\rp{-T}}
\newcommand{\dev}{\rs{dev}}
\newcommand{\sph}{\rs{sph}}
\newcommand{\hyd}{\rs{hyd}}
\newcommand{\iso}{\rs{iso}}
\newcommand{\vol}{\rs{vol}}
\newcommand{\vM}{\rs{vM}}
\newcommand{\sym}{\rs{sym}}
\newcommand{\dil}{\rs{dil}}
\renewcommand{\ln}[1]{\textrm{ln}\left(#1\right)}
\newcommand{\uft}{\mathfrak{\boldsymbol{u}}}
\newcommand{\Pext}{\mathcal{P}\rp{ext}}
\newcommand{\Pint}{\mathcal{P}\rp{int}}
\newcommand{\Cg}{\boldsymbol{C}}
\newcommand{\detC}{\det \Cg}
\newcommand{\F}{\bm{F}}
\newcommand{\s}{\bm{\sigma}}
\newcommand{\ie}{\rp{e}}
\newcommand{\ipl}{\rp{p}}
\newcommand{\e}{\bm{\varepsilon}}
\newcommand{\ee}{{\e}\ie}
\newcommand{\ep}{{\e}\ipl}
\newcommand{\E}{\bm{E}}
\newcommand{\Ee}{\E\ie}
\newcommand{\Ep}{\E\ipl}
\newcommand{\Fe}{\bm{\F}\ie}
\newcommand{\Fp}{\bm{\F}\ipl}
\newcommand{\Cge}{\tilde{\Cg}\ie}
\newcommand{\Cgp}{\Cg\ipl}
\newcommand{\Se}{\tilde{\bm{S}}\ie}
\newcommand{\Me}{\tilde{\bm{M}}\ie}
\newcommand{\Lp}{\tilde{\bm{L}}\ipl}
\newcommand{\Dp}{\tilde{\bm{D}}\ipl}
\newcommand{\sw}{\rs{sw}}
\newcommand{\td}[1]{\dot{#1}}
\newcommand{\tdd}[1]{\ddot{#1}}
$$

```{admonition} Todo
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
   - [ ] Ebener Verzerrungszustand
```

```{admonition} Lernziele
:class: important
- Wie werden mechanische Problemstellungen formuliert?
- Was sind die Unbekannten der Bilanzgleichungen?
- Welche Zutaten braucht man um ein lösbares Problem zu erhalten?
- Wie lauten die wesentlichen Bilanzgleichungen der Kontinuumsmechanik?
- In welche Kategorien werden Randbedingungen untergliedert?
- Wozu brauchen wir Materialmodelle?
- Welches sind die grundlegenden Materialmodelle der Wärmeleitung, Fluidtransport und der Strukturmechanik?
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
\td{\varrho} +  \div{\varrho {\bm{v}}} = 0
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

## Kinematik

Der Kinemtischen Bedingungen haben besonders im Bereich der Strukturmechanik und der Lösung der Impulsbilanz eine hervorgehobene Bedeutung. So ist zum Beispiel die Forderung vom ebenbleiben des Querschnitts in der Bernoulli Balkentheorie eine starke kinematische Restriktion, welche auf die bekannte Differentialgleichung der Durchbiegung $w(x)$ führt. 
Im Rahmen der linearen Kontinuumsmechanik wird die Dehnung $\bm{\epsilon}$ als symmetrischer Anteil des Verschiebungsgradienten $\grad{\bm{u}}$ verstanden:

```{math}
:label: linearStrain
\bm{\epsilon} = \frac{1}{2} \left(\grad{\bm{u}}\T + \grad{\bm{u}} \right) \; .
```

Dieses Dehnungsmaß wird oft auch als Ingenieurdehnung bezeichnet und sollte nur in einem Dehnungsbereich < 10% angewendet werden. Zudem ist hervorzuheben, dass Starrkörpertranslationen keine Ingenieurdehnung hervorrufen, Starrkörperrotationen hingegen sehrwohl. Aus diesem Grunde ist darauf zu achten, dass beim Auftreten größerer Rotationen stehts eine nichtlineare Theorie (2.Ordnung oder allgemein nichtlinear) verwendet wird.

Aufgrund der Symmetrie des Dehnungstensors erhalten wir **6** zusätzliche Bedingungen zur Lösung der Anfangsrandwertproblems der Impulsbilanz.

## Materialmodell

Was zur Schließung der mathematischen Problemstellung jetzt noch fehlt ist eine Relation zwischen der primären Feldgröße und ihrer zugeordneten Flussgröße (sekundäre Größe). Die oben beschriebenen Bilanzgleichungen sind physikalische Prinzipien die unabhängig vom Werkstoff Gültigkeit besitzen. Die spezifischen Werkstoffeigenschaften werden über Materialmodelle, sogenannte konstitutive Modelle, in das Anfangsrandwertproblem eingebracht. Die konstitutive Materialtheorie ist eine Wissenschaft für sich und soll in dieser Vorlesung nicht weiter behandelt werden. In der Strukturmechanik können Materialien entsprechend ihrer Eigenschaften eingeteilt werden in:

```{figure} ./images/Materialklassen.png
---
height: 500px
name: materialklassen
---
Klassifizierung von Materialmodellen anhand ihrer Systemantwort. Abbildung nach {cite}`haupt2013continuum`
```

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

```{code-cell} ipython3

```
