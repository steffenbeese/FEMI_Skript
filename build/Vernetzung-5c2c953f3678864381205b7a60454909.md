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


# Vernetzung

Die Vernetzung (Meshing) ist ein zentraler Schritt jeder FEM-Analyse. Ziel ist die Diskretisierung eines kontinuierlichen Berechnungsgebiets in endlich viele Elemente mit geeigneter Form, Größe und Topologie. Die Qualität dieses Netzes beeinflusst Genauigkeit, Konvergenz und Rechenzeit oft stärker als die spätere Wahl des Lösers.

## Einordnung und Grundbegriffe

Historisch wurden zunächst semi-automatische Verfahren eingesetzt, etwa isoparametrisches und transfinites Mapping sowie PDE-basierte Gittergenerierung. Diese Methoden sind leistungsfähig, setzen aber meist eine manuelle Blockzerlegung (Multi-Block) voraus.

Moderne FEM-Workflows nutzen zusätzlich vollautomatische Verfahren, die aus Randbeschreibung und Größenvorgaben direkt ein unstrukturiertes Netz erzeugen.

Man unterscheidet grundsätzlich:

- Strukturierte Gitter: regelmäßige Topologie, typischerweise Quad- (2D) oder Hexaederelemente (3D). Vorteilhaft für Genauigkeit und numerische Stabilität, aber bei komplexer Geometrie schwer automatisch zu erzeugen.
- Unstrukturierte Gitter: flexible Topologie, häufig Dreiecke (2D) bzw. Tetraeder (3D). Sehr robust bei komplizierten Konturen und damit im industriellen Alltag weit verbreitet.

## 2D-Vernetzung mit der Advancing Front Method

Die Advancing Front Method (AFM) startet von einer diskretisierten Randfront und arbeitet sich schrittweise in das Innere vor. Das Verfahren kombiniert Geometrienähe, gute lokale Elementformen und kontrollierte Größenveränderung.

Ein typischer Ablauf besteht aus drei Phasen:

1. Randknotenerzeugung: Diskretisierung der Randkurven in Frontsegmente.
2. Element- und Knotenerzeugung: Aufbau neuer Dreiecke ausgehend von einer aktiven Frontkante.
3. Qualitätsverbesserung: Glättung und topologische Optimierung.

Bei der Elementerzeugung wird häufig die kürzeste aktive Frontkante $ab$ gewählt. Aus ihrem Mittelpunkt wird eine ideale Kandidatenposition für den neuen Knoten bestimmt. Falls dort bereits ein geeigneter Frontknoten existiert, wird dieser genutzt; andernfalls wird ein neuer Knoten erzeugt. Anschließend werden Überschneidungen geprüft und die Front aktualisiert (verbrauchte Kanten entfernen, neue Kanten hinzufügen). Das Verfahren endet, wenn keine aktive Front mehr existiert.

![Schematischer Ablauf der Advancing Front Method](AdvancingFront.png)

*Abbildung: Elementerzeugung und Front-Update bei der Advancing Front Method.*

## Qualitätsverbesserung in 2D

Nach der reinen Netzgenerierung folgen oft gezielte Verbesserungsschritte:

- Laplacian Smoothing: Ein innerer Knoten wird in Richtung des Schwerpunkts seiner Nachbarn verschoben,

  $$
  \mathbf{x}_i = \frac{1}{N} \sum_{j=1}^{N} \mathbf{x}_j.
  $$

  Dadurch wird die Elementform oft ruhiger und gleichmäßiger. Bei konkaven Randbereichen sind jedoch Nebenbedingungen nötig, damit Knoten nicht aus dem gültigen Gebiet herauswandern.

- Topologische Modifikation: Knotenelimination und Diagonal Swapping verbessern Winkelverteilungen und Nachbarschaftsstrukturen. Ziel ist meist die Reduktion ungünstiger Innenwinkel und die Robustheit im späteren Lösungsschritt.

## 3D-Vernetzung mit Delaunay-Ideen

Für unstrukturierte 3D-Netze ist die Delaunay-Triangulierung ein wichtiges Prinzip. Das zentrale Kriterium lautet: Der Umkreis (2D) bzw. die Umkugel (3D) eines Elements soll im Inneren keine weiteren Knoten enthalten (Leere-Kreis/Leere-Kugel-Eigenschaft).

Ein typischer inkrementeller Ansatz ist Bowyer-Watson:

1. Neuer Punkt wird eingebracht.
2. Alle Elemente, deren Umkreis/Umkugel den Punkt enthält, werden als Kavität identifiziert.
3. Diese Elemente werden entfernt; anschließend wird der neue Punkt mit dem Kavitätsrand neu verbunden.

Das Verfahren ist mathematisch gut begründet und bildet in vielen Programmen den Kern der Tetraedernetz-Erzeugung.

![Delaunay-Kriterium und inkrementelle Triangulierung](Delaunay.png)

*Abbildung: Prinzip der Delaunay-Triangulierung mit leerem Umkreis/leerem Umkugel-Kriterium.*

## Strukturierte und unstrukturierte Hexaedernetze

Hexaeder sind in vielen strukturmechanischen Aufgaben sehr attraktiv, insbesondere bei biegungs- und schubdominierten Zuständen sowie bei hohen Anforderungen an die Spannungsqualität.

- Strukturierte Hex-Netze entstehen oft über Mapping- und Multi-Block-Strategien. Sie liefern häufig sehr gute Elementqualität, erfordern aber erheblichen manuellen Vorbereitungsaufwand.
- Unstrukturierte All-Hex-Verfahren (z. B. Octree-basierte Strategien, Plastering, Dualverfahren) sind algorithmisch deutlich anspruchsvoller. Die vollautomatische Erzeugung hochwertiger Hex-Netze für komplexe Geometrien bleibt ein aktives Forschungsthema.

## Elementwahl in der Praxis

Eine pauschal beste Elementfamilie gibt es nicht. Die Wahl hängt von Zielgröße, Lastfall, Geometrie und verfügbaren Ressourcen ab.

- Lineare Elemente: robust und oft günstig bei großen Verzerrungen oder Kontaktproblemen.
- Quadratische Elemente: meist bessere Spannungs- und Dehnungsabbildung, besonders bei biegedominierten Aufgaben.
- Tetraeder vs. Hexaeder: Hexaeder liefern bei vergleichbarer Netzqualität oft bessere Ergebnisse pro Freiheitsgrad; bei sehr komplexer Geometrie sind quadratische Tetraeder im Gesamtprozess häufig effizienter.

Eine belastbare Praxisregel lautet: Zuerst Modellierungsziel und Auswertegröße festlegen, dann Elementtyp und Vernetzungsstrategie bestimmen.

## Typische Fehler bei der Vernetzung

Häufige Problemursachen sind:

- schlechte Elementformen (stark verzerrt, ungünstige Winkel, entartete Elemente),
- nicht konforme Übergänge ohne geeignete Kopplung,
- unpassende Netzgröße für die relevante Physik (zu grob in Spannungsgradienten),
- fehlende oder ignorierte Elementqualitätschecks.

Insbesondere entartete Elemente können Spannungsauswertungen massiv verfälschen. Deshalb sollten Qualitätsmetriken (z. B. Winkel, Aspect Ratio, Jacobian-bezogene Kriterien) bereits vor der Berechnung systematisch geprüft werden.

## Zusammenfassung

Die Vernetzung ist kein reiner Preprocessing-Nebenschritt, sondern ein wesentlicher Bestandteil der Modellqualität. Gute Netze entstehen durch das Zusammenspiel aus:

- geeigneter Netzstrategie (strukturiert/unstrukturiert),
- passender Elementwahl (linear/quadratisch, Tet/Hex),
- kontrollierter Größensteuerung,
- und konsequenter Qualitätssicherung vor dem Lösen.

Damit wird die Grundlage geschaffen, dass FEM-Ergebnisse nicht nur numerisch stabil, sondern auch ingenieurmäßig belastbar sind.

