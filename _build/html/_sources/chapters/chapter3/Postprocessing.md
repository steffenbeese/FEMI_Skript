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

# Postprocessing

Das Postprocessing stellt einen unverzichtbaren Schritt in der Finite-Elemente-Analyse (FEA) dar. Es ist definiert als der Prozess der Transformation und Aufbereitung der oft hochdetaillierten und komplexen Rohausgaben von FEM-Berechnungen in ein für den Anwender leicht verständliches Format. Im Wesentlichen handelt es sich um die Modifikation oder Verbesserung von Ergebnisdaten, nachdem eine Lösung aus einem Rechenmodell erhalten wurde, mit dem Ziel, aussagekräftige Visualisierungen, Diagramme und andere Ausgaben zu erstellen.

```{figure} images/postproc_ansys.png
---
name: postprocansys
alt: postprocansys
width: 500px
---
Postprocessing in ANSYS Mechanical nach [link](https://www.padtinc.com/simulation/simulation-product/ansys-mechanical/).
```



## Postprocessing primärer Größen

In der Finite-Elemente-Analyse werden die sogenannten primären Größen direkt als Ergebnis der Lösung des globalen Gleichungssystems berechnet. Typische Beispiele hierfür sind Verschiebungen in der Strukturmechanik oder Temperaturen in der Wärmeübertragung. Diese Größen werden explizit an den Knoten des Finite-Elemente-Netzes ermittelt. Die resultierenden Knotenverschiebungen gelten als präzise, da jedem Knoten innerhalb des Netzes ein einziger, eindeutiger Verschiebungswert zugewiesen wird. Dies unterscheidet sich grundlegend von den sekundären Größen, die später erläutert werden.  
Die Werte der primären Feldvariablen, die an den diskreten Knoten berechnet werden, dienen dazu, die Werte an allen anderen Punkten innerhalb eines Elements  zu approximieren. Dies geschieht durch Interpolation der Knotenwerte mittels der Formfunktionen des Elementes.
Dies gewährleistet die Inter-Element-Kontinuität der primären Feldvariablen (z.B. Verschiebung oder Temperatur) an den Knotenpunkten. Die Formfunktionen stellen somit die mathematische Brücke dar, die es ermöglicht, die diskrete Lösung an den Knoten in ein kontinuierliches Feld innerhalb jedes Elements zu überführen. Diese Interpolationsfähigkeit ist von grundlegender Bedeutung für die FEM, da sie es erlaubt, mit einer endlichen Anzahl von Freiheitsgraden ein kontinuierliches physikalisches Phänomen anzunähern. 

| Kriterium | Primäre Größen (z.B. Verschiebung, Temperatur) | Sekundäre Größen (z.B. Spannung, Dehnung, Moment) |
| :---- | :---- | :---- |
| **Typ** | Feldvariablen, direkte Lösung | Abgeleitete Größen |
| **Berechnungspunkt** | Knoten | Gaußpunkte (dann zu Knoten extrapoliert) |
| **Kontinuität** | C0-kontinuierlich (an Knoten) | Diskontinuierlich an Elementgrenzen |
| **Genauigkeit (relativ)** | Präzise (eindeutiger Wert pro Knoten) | Weniger präzise (aber genauer an Gaußpunkten) |
| **Rolle im Postprocessing** | Direkte Ausgabe, Visualisierung | Erfordert Glättung für physikalische Interpretation |

## Postprocessing sekundärer Größen 

```{figure} images/FEM_Postprocessing.png
---
name: postprocFEM
alt: postprocFEM
width: 300px
---
Unterschied zwischen primären und sekundären Größen im Postprocessing.
```

Im Gegensatz zu primären Größen werden sekundäre Größen, wie Spannungen, Dehnungen und Momente, nicht direkt an den Knoten berechnet. Stattdessen werden diese abgeleiteten Größen an den sogenannten Gauß-Integrationspunkten innerhalb jedes Finite-Elements ermittelt.  
Der Grund für die Berechnung an Gaußpunkten liegt in der mathematischen Formulierung der FEM, insbesondere im Rahmen der Galerkin-Methode. Die Berechnung von Spannungen und Dehnungen erfolgt aus den Ableitungen der Verschiebungsfelder. Die Gaußpunkte sind die optimalen Punkte für die numerische Integration der Steifigkeitsmatrizen und Lastvektoren und bieten die genauesten Stellen zur Bestimmung dieser abgeleiteten Größen. Der Element-Solver "kennt" die Dehnung innerhalb des Elements basierend auf den Knotenverschiebungen und kann daraus die Spannung berechnen. Es ist jedoch wichtig zu verstehen, dass das Element kein "vollständiges Wissen" über das Geschehen in seinem Inneren besitzt; es prognostiziert lediglich einige korrekte Antworten an den Gaußpunkten und schätzt bzw. extrapoliert diese wenigen genauen Antworten auf den Rest seiner Fläche.  
Um die an den Gaußpunkten berechneten Werte der sekundären Größen an den Knotenpositionen zu erhalten, ist eine Extrapolation dieser Werte von den Gaußpunkten zu den Knoten erforderlich. Diese Extrapolation ist notwendig, da die Knoten oft die primären Punkte für die Ergebnisdarstellung und \-interpretation sind.  
Es existieren verschiedene Methoden zur Extrapolation von Gaußpunktwerten zu den Knoten, die jeweils unterschiedliche Implikationen für die Ergebnisdarstellung und \-genauigkeit haben:

- **Zentroidaler Wert (Centroidal Value):** Bei dieser einfachen Methode wird der Durchschnitt der Gaußpunkt-Ergebnisse über alle Gaußpunkte eines Elements berechnet und dieser gemittelte Wert allen Knoten des jeweiligen Elements zugewiesen. Dies führt zu einer sehr starken Glättung der Ergebnisse innerhalb eines Elements.  

- **Nodalwerte, extrapoliert von Gaußpunkten (Nodal Values Extrapolated from Gauss Points):** Diese Methode verwendet lineare Formfunktionen, um die Gaußpunktwerte zu den Knoten zu extrapolieren. Die Extrapolation entspricht einem Least-Squares-Fit. Sie wird im Allgemeinen für die Mehrheit der linearen Materialmodelle bevorzugt. Eine wichtige Einschränkung ist jedoch, dass diese Option für nichtlineare Materialien unter Umständen nicht gültig ist, da die Extrapolation von Gaußpunktspannungen zu den Knoten zu nodal Spannungen führen könnte, die die Materialgrenzen überschreiten, was physikalisch inkorrekt wäre.

- **Gaußpunktwerte direkt an Knoten platziert (Gauss Point Values Placed at Nodes):** Hierbei wird jedes Gaußpunkt-Ergebnis direkt seinem nächstgelegenen Knoten zugewiesen, ohne weitere Extrapolation. Diese Option ist in der Regel am zuverlässigsten für nichtlineare Materialien, da die direkt zugewiesenen Knotenwerte niemals die Materialgrenzen überschreiten werden.

Die grundlegende Natur sekundärer Größen als "abgeleitete" Größen hat weitreichende Auswirkungen auf ihre Genauigkeit. Während primäre Größen direkt aus der Lösung der Gleichgewichtsgleichungen resultieren , werden sekundäre Größen durch Rücksubstitution aus den primären Größen berechnet. Da die FEM eine Näherungstechnik ist, propagieren Fehler in den primären Größen und können sich in den abgeleiteten sekundären Größen sogar verstärken, was zu einer höheren Fehleranfälligkeit führt (Siehe Konvergenzanalyse der Querkraft bei der Balken FEM). Die Wahl der Extrapolationsmethode ist somit nicht willkürlich, sondern hängt maßgeblich vom Materialmodell ab. Eine lineare Extrapolation kann für nichtlineare Materialien physikalisch unmögliche Ergebnisse liefern, was eine konservativere, direkte Zuweisung erforderlich macht. Dies verdeutlicht die Notwendigkeit, das zugrunde liegende Materialverhalten und dessen Wechselwirkung mit den numerischen Methoden genau zu verstehen, da dies die Gültigkeit und den physikalischen Realismus der postprozessierten Ergebnisse direkt beeinflusst.

| Methode | Prinzip | Anwendungsbereich / Vorteile | Nachteile / Überlegungen |
| :---- | :---- | :---- | :---- |
| **Zentroidaler Wert** | Durchschnitt aller Gaußpunktwerte eines Elements wird allen Knoten zugewiesen. | Einfache, schnelle Glättung. | Stark geglättet, verliert lokale Details. |
| **Nodalwerte, extrapoliert von Gaußpunkten** | Lineare Formfunktionen extrapolieren Gaußpunktwerte zu Knoten. | Bevorzugt für lineare Materialmodelle. | Kann bei nichtlinearen Materialien Materialgrenzen überschreiten. |
| **Gaußpunktwerte direkt an Knoten platziert** | Jedes Gaußpunkt-Ergebnis wird direkt dem nächstgelegenen Knoten zugewiesen. | Am zuverlässigsten für nichtlineare Materialien, da Materialgrenzen nicht überschritten werden. | Führt zu Diskontinuitäten an Elementgrenzen, da keine Glättung erfolgt. |

```{admonition} Fragen zum Kapitel
:class: warning


**Postprocessing**

- Was sind primäre Größen der FEM in der Strukturmechanik und in der Wärmeleitung? 
- Wo liegen die primären Größen vor?
- Wo liegen die sekundären Größen bei der FEM vor?
- Warum werden sekundäre Größen an Gauß-Integrationspunkten berechnet und nicht direkt an den Knoten?
- Welche Methoden existieren, um sekundäre Größen von Gaußpunkten zu den Knoten zu extrapolieren?
- Was ist ein Indikator für eine konvergierte Lösung einer sekundären Größe?
```