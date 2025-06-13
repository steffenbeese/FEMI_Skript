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
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

# Numerische Modelle in der Mechanik

Die numerische Mechanik bedient sich computergestützter Techniken zur Ermittlung von Näherungslösungen für Differentialgleichungen, die physikalische Problemstellungen beschreiben. Es wird zwischen zwei Hauptarten der Modellbildung unterschieden:

- Diskrete Modelle
- Kontinuierliche Modelle

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Diskrete Modelle

Diskrete Modelle der Mechanik basieren auf massebehafteten, starren Massepunkten oder Körpern, die durch Kraftpotentiale oder Federverbindungen miteinander in Wechselwirkung stehen. Solche Potentiale werden verwendet, um Interaktionen zwischen atomaren Partikeln (Molekulardynamik) oder Planeten (Gravitationsgesetze) zu modellieren. 

Ein weiteres Anwendungsgebiet in der diskreten Modellierung der Mechanik sind Mechanismen: Systeme aus kinematisch verbundenen, starren Körpern, verbunden durch Kontakte und diskret modellierte elastische Feder- und Dämpferelementen. Beispiele hierfür sind industrielle Fertigungsmaschinen oder der Mechanismus eines Kugelschreibers, von Spannelementen oder Fahrwerken.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{figure} ./images/MKS_Robot.png
---
height: 300px
name: MKS
---
Mehrkörperproblem eines zweiarmigen Roboters nach {cite}`featherstone2014rigid`
```

Die mathematische Beschreibung solcher Modelle resultiert typischerweise in einem System gekoppelter Bewegungsgleichungen:


\begin{equation}
\boldsymbol{M} \ddot{\boldsymbol{x}} + \boldsymbol{D} \dot{\boldsymbol{x}} + \boldsymbol{K} \boldsymbol{x} = \boldsymbol{f}(t)
\end{equation}

Hierbei repräsentiert $\boldsymbol{x}$ die Position des Massepunktes im Raum und $\ddot{\boldsymbol{x}}$ dessen Beschleunigung. Die Trägheitsmatrix $\boldsymbol{M}$ approximiert die Massenträgheit der Körper, während die Steifigkeitsmatrix $\boldsymbol{K}$ die elastischen Bindungen darstellt. Die Matrix $\boldsymbol{D}$ führt Dämpfungsterme in das System ein und $\boldsymbol{f}(t)$ sind äußere zeitabhängige Kräfte.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Bei Mechanismen kommen oft Nebenbedingungen wie Kontaktbedingungen oder Regelungsalgorithmen hinzu, was zu Differential-Algebraischen-Gleichungssystemen (DAE) führt.

Die Herausforderung in der diskreten Mechanik liegt in der Modellierung: Das System muss abstrahiert, Gleichungen und Bedingungen formuliert und für den Rechner aufbereitet werden. Die numerische Aufgabe besteht dann hauptsächlich darin, die Bewegungsgleichungen über die Zeit zu integrieren. In der Festkörpermechanik sind zwei Methoden etabliert:

- Molekular-Dynamik Simulation (MDS) {cite}`grotendorst2009multiscale`
- Mehr-Körper-Simulation (MKS) {cite}`shabana1997flexible`

Für strömungsmechanische Fragestellungen sind ebenfalls Partikelmethoden wie die Gitter-Boltzmann-Methode etabliert.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Anwendungen für MKS

Die hier gezeigten Beispiele stammen aus der freien MKS-Software der TU-München [MBSim](https://www.mbsim-env.de/)

<iframe width="640" height="480" src="https://www.mbsim-env.de/static/home/videos/xml_planetary_gear.317ba38de10c.webm" frameborder="0" allowfullscreen></iframe>

**Mehrkörpersimulation eines Planeten Getriebe**

<iframe width="640" height="480" src="https://www.mbsim-env.de/static/home/videos/xml_spinning_plate.c5e942e12401.webm" frameborder="0" allowfullscreen></iframe>

**Mehrkörpersimulation eines rotierenden Tellers**

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


## Kontinuierliche Modelle

Die Modellbildung in diesem Bereich geht von einem gleichförmigen (homogenen) Materialverhalten aus. Feinheiten der komplexen atomaren Interaktionen, wie sie beispielsweise in Kristallstrukturen auftreten und auf sehr kleinen zeitlichen sowie räumlichen Skalen stattfinden, werden hierbei im Sinne einer statistischen Mittelung vereinfacht dargestellt. Die zugrundeliegende Vorstellung ist die eines Kontinuums, dessen Charakteristika mithilfe von partiellen Differentialgleichungen (Feldgleichungen) räumlich und zeitlich erfasst werden können. Dieses Feld gehört zur Disziplin der Kontinuumsmechanik.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

In dieser Lehrveranstaltung konzentrieren wir uns auf die Behandlung dieser Feldgleichungen und ihre näherungsweise Lösung durch numerische Verfahren, insbesondere für Fragestellungen der linearen Elastizitätslehre. Es sollte jedoch erwähnt werden, dass vergleichbare Feldtheorien auch in anderen Bereichen, wie zum Beispiel bei elektromagnetischen Feldern, Anwendung finden und die Methoden entsprechend adaptiert werden können.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Unser Fokus liegt auf der Finite-Elemente-Methode (FEM), wobei auch die Finite-Differenzen-Methode am Rande behandelt wird. Die Finite-Elemente-Methode gilt aktuell als das effektivste Verfahren zur näherungsweisen Lösung partieller Differentialgleichungen in der Festkörpermechanik. Kommerzielle Softwarelösungen sind weit verbreitet und werden industriell genutzt. Ein wesentliches Lernziel dieses Kurses ist es daher, ein fundiertes Verständnis für die Finite-Elemente-Methode zu entwickeln, um sie sachgemäß in der Praxis anwenden zu können.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

In der Fluidmechanik hat sich die FEM aufgrund der besonderen Beschaffenheit der Differentialgleichungen nicht durchgesetzt, was ebenfalls thematisiert wird. In diesem Gebiet sind traditionell die Finite-Differenzen-Methode und heute vermehrt die Finite-Volumen-Methode gebräuchlich.


```{admonition} Fragen zum Kapitel
:class: warning

**Diskrete Modell**

- Wie treten Körper innerhalb der diskreten Mechanik in Interaktion?
- Welche Gleichung ist die Grundlage der Simulation von  Mehrkörpersystemen?
- Welche zwei Methoden sind in der diskreten Mechanik etabliert?
- Was ist die numerische Herausforderung in der diskreten Mechanik?
- Welche ingeneur-technische Herausforderung gilt es in der diskreten Mechanik zu bewältigen?

**Kontinuierliche Modelle**
- Welcher Gleichungstyp liegen den kontinuierlichen Modellen der Mechanik zugrunde?  
- Was ist korrekt?
  - [ ] Die kontinuierlichen Modelle sind die Grundlage der FEM.
  - [ ] In den kontinuierlichen Modellen werden komplexe zwischen-atomare Interaktionen berücksichtigt.
  - [ ] Das Material wird als homogen angesehen.
  - [ ] Die FEM ist besonders für die Fluidmechanik geeignet. 

```