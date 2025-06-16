
+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

# Isoparametrische FEM

```{figure} images/Nackenhorst_01.png
---
width: 600px
name: isogeom
---
Geometrieapproximation am Beispiel der Diskretisierung eines Eisenbahnrades {cite}`numerische_mechanik_2022`.
```

Die isoparametrische Finite Elemente Methode ist heute der geläufige Standard in FEA Software. Erstmals erwähnt wurde die isoparameterische Finite Elemente Methode von I.C. Taig (britischer Luftfahrtingenieur) im Jahre 1958. Die Methode bildet die Grundlage um komplizierte Geometrien mit gekrümmten Rändern zu modellieren. Als Beispiel sei hier die Diskretisierung in Abbildung {numref}`isogeom` eines Eisenbahnrades gezeigt. In den nächsten Abschnitten werden einige Aspekte der isoparametrischen Finite Elemente Methode erläutert. Für einen umfassender Einblick sei auf die Standardwerke von {cite}`zienkiewicz2005finite`, {cite}`hughes2003finite` oder  {cite}`bathe2006finite` verwiesen.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Begriffsklärung

In der schwachen Form {eq}`weakform_03` werden die Felder $\bm{u}_h$, $\delta\bm{u}_h$ und die Koordinaten $\bm{x}$, sowie deren räumliche Ableitungen berechnet. Das Aufstellen der Ansatzfunktionen für diese Felder ist für komplexe Geometrien nicht trivial. Daher werden die Ansatzfunktionen in einem sog. Elternelement definiert. Dieses wird dann auf die reale Geometrie abgebildet. Diese Abbildung wird als isoparametrische Abbildung bezeichnet. Die Abbildung wird durch die sog. Formfunktionen $\bm{N}$ beschrieben. Diese Formfunktionen sind in der Regel Polynome und werden in der Regel in einem sog. Elternelement definiert. Dieses Elternelement ist ein einfaches geometrisches Objekt, wie z.B. ein Einheitsquadrat oder ein Einheitsdreieck. Die Formfunktionen sind in diesem Elternelement definiert und werden dann auf die reale Geometrie abgebildet. Dies ist in Abbildung {numref}`IsoparamtericMapping` für den zweidimensionalen Fall eines Rechteckelementes dargestellt.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{figure} images/Taylor_isoQuad.png
---
width: 600px
name: IsoparamtericMapping
---
Abbildung des Elternelementes auf die reale Geometrie ({cite}`zienkiewicz2005finite`).
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Die Abbildung des Elternelementes auf die reale Geometrie entspricht einer Koordinaten-Transformation. Das Elternelement wird mit den Koordinaten $\xi$, $\eta$ und $\zeta$ beschrieben, während die reale Geometrie mit den Koordinaten $x$, $y$ und $z$ beschrieben wird. Die Transformation wird durch die Abbildung $\bm{x}_h = \bm{x}_h(\xi, \eta, \zeta)$ beschrieben:

```{math}	
:label: geometric_mapping
\begin{equation}
\bm{x}_h = \sum_{i=1}^{n} N_I(\bm{\xi}) \hat{\bm{x}}_I
\end{equation}
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

hierbei ist $\hat{\bm{x}}_I$ der Vektor der Knotenkoordinaten und $N_I(\bm{\xi})$ die Formfunktionen des Elternelementes. Die Formfunktionen sind in der Regel Lagrange-Polynome, die die Knoten des Elternelementes interpolieren. Die Formfunktionen sind somit stückweise linear, quadratisch oder kubisch, je nach Art des Elternelementes.

Wird für die Testfunktion $\delta \bm{u}_h$ und die Verschiebung $\delta\bm{u}_h$, sowie die diskrete Darstellung der Geometrie $\bm{x}_h$ der gleiche Ansatz verwendet, so spricht man von einem isoparametrischen Element:

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{math}
:label: isoparametric
\begin{align}
  \delta \bm{u}_h &= \sum_{I=1}^{n} N_I(\bm{\xi}) \delta \hat{\bm{u}}_I \\
  \bm{u}_h &= \sum_{i=1}^{n} N_I(\bm{\xi}) \hat{\bm{u}}_I \\
  \bm{x}_h &= \sum_{i=1}^{n} N_I(\bm{\xi}) \hat{\bm{x}}_I
\end{align}
```

```{admonition} Abgrenzung: weitere Parameterformulierungen
:class: tip
Neben der isoparametrischen Formulierung gibt es auch andere Parameterformulierungen:
  - subparametrische Formulierung: Geometrie wird mit niedrigerem Polynomgrad als die Verschiebung interpoliert
  - superparametrische Formulierung: Geometrie wird mit höherem Polynomgrad als die Verschiebung interpoliert
Beide Konzepte haben sich in der Praxis nicht durchgesetzt, wobei die superparametrische generell vermieden werden sollte (Keine Konvergenz sichergestellt)
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Berechnung der räumlichen Ableitungen

Möchte man ein diskretisiertes Feld z.B. $\bm{u}_h$ ableiten, so muss man die über die Kettenregel die Formfunktionen ableiten:

```{math}
:label: ableitungFormfunktion
\begin{align}
\Pd{\bm{u}_h}{\bm{x}} = \sum_{I=1}^{n} \Pd{N_I}{\bm{\xi}} \Pd{\bm{\xi}}{\bm{x}}  \hat{\bm{u}}_I \;.
\end{align}
```

In dieser Gleichung ist $\Pd{\bm{\xi}}{\bm{x}}$ jedoch unbekannt. Über die parametrische Darstellung der Geometrie {eq}`geometric_mapping` kann man jedoch die Ableitung $\Pd{\bm{\xi}}{\bm{x}}$ berechnen:
```{math}
:label: ableitungFormfunktion_02
\begin{align}
\bm{J} := \Pd{\bm{x}}{\bm{\xi}} & = \begin{pmatrix}
\Pd{x}{\xi} & \Pd{x}{\eta} & \Pd{x}{\zeta} \\
\Pd{y}{\xi} & \Pd{y}{\eta} & \Pd{y}{\zeta} \\
\Pd{z}{\xi} & \Pd{z}{\eta} & \Pd{z}{\zeta} 
\end{pmatrix}
=  \sum_{I=1}^{n} \Pd{N_I}{\bm{\xi}} \hat{\bm{x}}_I \\
\Rightarrow \Pd{\bm{\xi}}{\bm{x}} & = \bm{J}^{-1} \; .
\end{align}
```
Hier wurde die **Jacobi-Matrix** $\bm{J}$ eingeführt. Zur Berechnung der räumlichen Ableitungen der Formfunktionen werden diese einfach mit der inversen Jacobi-Matrix multipliziert:

```{math}
:label: ableitungFormfunktion_03
\Pd{N_I}{\bm{x}} = \Pd{N_I}{\bm{\xi}} \Pd{\bm{\xi}}{\bm{x}} = \Pd{N_I}{\bm{\xi}} \bm{J}^{-1} \; .
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{admonition} Bedeutung der Jacobi Matrix
:class: tip
Die Determinante der Jacobi-Matrix $\det(\bm{J})$ wird als **Jacobian** bezeichnet und stellt ein wichtiges Maß für die Qualität der Elementform dar. Eine Jacobi-Determinante von Null bedeutet, dass die Elementform singulär ist und die Ableitung der Formfunktionen nicht mehr eindeutig definiert ist. Es ist daher wichtig, dass die Jacobi-Determinante in der gesamten Element positiv ist. Eine Jacobi-Determinante in der Nähe von 1 ist ideal, während Werte weit von 1 entfernt auf verzerrte Elemente hinweisen. Negative Werte der Jacobi-Determinante sind ebenfalls problematisch und sollten vermieden werden. Hier hat sich das Element selbst durchdrungen. Solche Elemente fügen dem System Energie hinzu und verfälschen die Lösung maßgeblich.
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Anforderungen an den Finite Elemente Ansatz


Um die Finite-Elemente-Methode (FEM) effektiv anzuwenden, müssen die Formfunktionen bestimmte Anforderungen erfüllen:

**Kontinuität**: 

Die Formfunktionen müssen im gesamten Bereich stetig vom Grad $C^{n-1}$ sein, wobei $n$ die höchste Ableitung ist, die in der schwachen Formulierung {eq}`weakform_03` vorkommt. Beispielsweise erfordert die Elastizitätstheorie $n=1$, was bedeutet, dass der Ansatz $C^0$-stetig sein muss. Insbesondere muss diese Stetigkeit über die Elementgrenzen hinweg gewährleistet sein.
Beim Balkenelement trat die Krümmung (2. Ableitung der Durchbiegung) in der schwachen Form auf. Deshalb muss der Ansatz für das Balkenelement $C^1$-stetig sein.


+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

**Vollständigkeit**: 

Die Formfunktionen müssen die exakte Lösung approximieren können, wenn die Anzahl der Elemente gegen unendlich geht. Dies stellt sicher, dass die numerische Lösung mit zunehmender Verfeinerung des Netzes immer genauer wird.

**Eindeutigkeit**: 

Die Formfunktionen müssen eindeutig sein, d.h., es darf keine lineare Abhängigkeit zwischen den Formfunktionen geben. Diese Eigenschaft ist entscheidend, um sicherzustellen, dass die Lösung der Gleichungen eindeutig bestimmt ist.

Diese Anforderungen sind essenziell, um die Genauigkeit und Zuverlässigkeit der FEM-Lösungen zu gewährleisten. Sie stellen sicher, dass die numerischen Ergebnisse physikalisch sinnvoll und mathematisch korrekt sind und dass die Methode gegen die analytische Lösung konvergiert. 

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Für diese drei Anforderungen existiert auch eine Ingenieurmäßige Deutung, welche im Folgenden beschrieben wird. 

```{figure} images/NonConforming_Mesh_01.png
---
width: 600px
name: Stetigkeit_01
---
Verletzung der Stetigkeitsanforderung in der Finiten Elemente Methode.
```

Die Forderung der Stetigkeit über die Elementgrenzen hinweg ist wird über eine konforme Vernetzung sichergestellt. Ein typisches Beispiel zur Veranschaulichung dieser Anforderung ist die Elementierung mit 4-Knoten-Rechteckelementen.
Betrachten wir eine eingespannte Scheibe, die einer Zugbelastung ausgesetzt ist. Um die Schnittkräfte genauer zu erfassen, wird das Netz in Richtung der Einspannung verfeinert. Dies soll die Behinderung der Querkontraktion besser erfassen. Allerdings führt diese Netzverfeinerung in einigen Fällen zu unzulässigen Klaffungen im deformierten Netz. 

Die Ursache für diese unzulässigen Klaffungen liegt in der Zuordnung der Knoten zu den Elementen. In diesem Beispiel werden die Knoten 7,9 und 12 nur zu zwei Elementen zugeordnet. Die Elementkante von Element 5, 6 und 7 wird nicht an die Deformation der genannten Knoten gekoppelt. Diese fehlerhafte Zuordnung führt dazu, dass die Schnittkräfte in der Nähe von Inkompatibilitäten nicht korrekt berechnet werden.

```{figure} images/NonConforming_Mesh_02.png
---
width: 600px
name: Stetigkeit_02
---
Verschiebungsfeld bei einer nicht-konformen Netzstruktur.
```


Diese lokalen Störungen klingen jedoch mit ausreichend großem Abstand ab, wie es das Prinzip von St. Venant beschreibt. Dieses Prinzip besagt, dass die Auswirkungen von lokalen Unregelmäßigkeiten in der Belastung oder Geometrie mit zunehmendem Abstand von der Störungsquelle abnehmen. Daher sind die Auswirkungen der Inkompatibilitäten auf die Gesamtlösung begrenzt.

Ein weiterer Fehler, der bei der Vernetzung entstehen kann, ist die Verwendung von Elementen mit unterschiedlicher Elementordnung.

```{figure} images/NonConforming_Mesh_03.png
---
width: 400px
name: Stetigkeit_03
---
Verletzung der Stetigkeitsanforderung in der Finiten Elemente Methode durch Verwendung von Elementen mit unterschiedlicher Ordnung.
```

Auch hier ist die resultierende Verschiebung nicht korrekt. Eigentlich sollte die Verschiebung linear sein, was jedoch nicht der Fall ist.

```{figure} images/NonConforming_Mesh_04.png
---
width: 600px
name: Stetigkeit_04
---
Verletzung der Stetigkeitsanforderung in der Finiten Elemente Methode durch Verwendung von Elementen mit unterschiedlicher Ordnung - Resultierendes Verschiebungsfeld.
```

Die Stetigkeitsanforderung ist von großer praktischer Bedeutung, da sie sicherstellt, dass die numerische Lösung physikalisch sinnvoll und mathematisch korrekt ist. Eine konforme Vernetzung, bei der die Knoten korrekt zugeordnet sind, ist entscheidend, um die Genauigkeit der FEM-Lösungen zu gewährleisten. 


Um eine lokale Verfeinerung zu erzielen kann man entweder spezielle Elemente verwenden (bei denen die entsprechenden Knoten angelegt wurden) oder man fügt eine entsprechende Zwischenschicht wie in Abbildung {numref}`Stetigkeit_05` ein.

```{figure} images/Conforming_Mesh_01.png
---
width: 600px
name: Stetigkeit_05
---
Übergangselementschicht zur lokalen Verfeinerung.
```

Hierbei wird das Verschiebungsfeld durch die Zwischenschicht korrekt wiedergegeben.

```{figure} images/Conforming_Mesh_02.png
---
width: 600px
name: Stetigkeit_06
---
Übergangselementschicht zur lokalen Verfeinerung - Verschiebungsfeld.
```




+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Ein zentraler Aspekt der Vollständigkeit in der Finite-Elemente-Methode ist die Fähigkeit, Starrkörperverschiebungen darzustellen. Die Starrkörperverschiebung $\bm{u}_c = \bm{c}$ muss durch die Ansatzfunktionen $\bm{N}_I(\bm{\xi})$ exakt wiedergegeben werden können. Der mathematische Ausdruck dafür ist:
```{math}
:label: completeness
\begin{align}
\bm{u}_c & = \sum_{I=1}^{n} N_I(\bm{\xi}) \hat{\bm{u}}_I  = \sum_{I=1}^{n} N_I(\bm{\xi}) {\bm{c}} = \bm{c} \sum_{I=1}^{n} N_I(\bm{\xi}) \\
\Rightarrow \qquad 1 &= \sum_{I=1}^{n} N_I(\bm{\xi}) 
\end{align}
```

Diese Eigenschaft wird in der Literatur als **Partitions of Unity** bezeichnet. Die praktische Relevanz dieser Eigenschaft liegt darin, dass in Tragstrukturen die Lösung in einen Anteil der Starrkörperverschiebung und einen Anteil, der Dehnungen hervorruft, aufgeteilt werden kann. Ein Beispiel hierfür ist ein Balken: Am freien Ende ist die Verschiebung am größten, der Balken jedoch erfährt hier am wenigsten Dehnung. Eine praktische Überprüfung besteht darin, dass Starrkörperverschiebungen keine Dehnungen/Spannungen hervorrufen dürfen.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Ein weiteres Beispiel zur Überprüfung der Vollständigkeit ist die Darstellbarkeit konstanter Dehnungszustände (Patch-Test). Ist ein Element in der Lage, einen konstanten Dehnungszustand darzustellen, so ist auch ein praktischer Nachweis der Konvergenz der Elementformulierung bei Verfeinerung der Vernetzung gegeben. Dies kann man sich einfach am Beispiel in Abbildung {numref}`KonvergenzPatchtest` verdeutlichen. Hier werden die Schnittkräfte in einen konstanten Anteil und einen linear veränderlichen unterteilt. Mit steigender Elementanzahl wird der konstante Term der Schnittkraft dominant und ist somit der maßgebliche Anteil für die Konvergenz.

```{figure} images/Knothe_02.png
---
width: 600px
name: KonvergenzPatchtest
---
Mit steigender Elementanzahl wird der konstante Term der Schnittkraft dominant ({cite}`knothe1991finite`).
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}


```{admonition} Fragen zum Kapitel
:class: warning


**Isoparametrische FEM**

- Welche Aussagen sind zutreffend?
  - [ ] Bei der isoparametrischen FEM wird die schwache Form auf einem uniformen Elternelement gebildet und danach auf die reale Geometrie transformiert.
  - [ ] Bei der isoparametrischen FEM werden die gleichen Ansätze bzgl. Geometrie und Verschiebung gewählt.
  - [ ] Bei der subparamterischen FEM wird die Geometrie mit einem niedrigeren Polynomgrad als die Verschiebung approximiert.
  - [ ] Nur die isoparametrische FEM ist gesichert konvergent.
- Über welches Maß kann man die Qualität eines FE-Netzes quantifizieren?
- Ist ein Element mit einer Jacobi-Determinante $\det \boldsymbol{J} < 0 $ für die Berechnung zulässig? 
- Welche Anforderungen muss ein Finite-Element-Ansatz erfüllen, dammit die Lösung gegen die analytische Lösung konvergiert?
- Wie kann man die Forderung der Kontinuität des Ansatzes bei der Vernetzung Verletzen?
- Warum ist es wichtig, dass ein Finites Element Starrköperverschiebungen korrekt darstellt?
- Was ist ein ein Patch-Test? Was wird damit untersucht?
- Was bedeutet der Begriff *Partition of Unity*? 

```

