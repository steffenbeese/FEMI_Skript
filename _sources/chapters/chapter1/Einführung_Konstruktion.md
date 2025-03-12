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
# Numerische Simulation im Konstruktionsprozess

```{figure} images/Konstruktionsprozess_knothe.png
---
height: 300px
name: konstruktionsprozess
---
Einordnung numerischer Simulation in den Konstruktionsprozess nach {cite}`knothe1991finite`
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Finite-Elemente-Programme spielen eine wichtige Rolle bei der Untersuchung realer Tragwerke, die bestimmten technischen Aufgabenstellungen unterliegen. Dieses leistungsfähige Tool ermöglicht es beispielsweise, die Integrität einer Pkw-Karosserie oder eines Rahmentragwerks zu analysieren, indem sie die Simulation eines Crash-Vorgangs durchführen oder die Traglast eines Bauteils ermitteln. Für die virtuelle Produktentwicklung und eine prototypenfreie (oder prototypenarme) Entwicklung ist die Finite Elemente Methode aus den Entwicklungsabteilungen nicht mehr wegzudenken.

Der Weg zu einer validen FE-Simulation ist mehrstufig und interdisziplinar. In der Abbildung {numref}`konstruktionsprozess` sind die eigentlichen Schritte, welche die Finite Elemente Methode betrifft durch einen dicken Rahmen gekennzeichnet. Die vorgelagerte Prozesskette sollte von dem/der berechnenden Ingenieur/in eng begleitet werden, denn die Ergebnisse eine Simulation können nur so gut sein wie die Eingangsdaten: **Garbage in, garbage out**.

Zu erst wird die reale Struktur und ihre Belastung definiert und anschließend in geeigneter Weise abstrahiert:
  - genügt eine 2D Berechnung oder gar eine 1D Berechnung
  - Welche geometrischen Details fließen in die Simulation ein
  - Kann ich die tatsächliche Last abstrahieren?
  - etc.

Das resultierende mechanische Modell ist somit eine idealisierte Darstellung der realen Problemstellung. Es geht darum, unwichtige Details wegzulassen, um die Berechnungen effizient und fokussiert zu halten, während gleichzeitig alle relevanten Belastungen präzise definiert werden müssen.

Auf die Modellierung folgt die Diskretisierung in finite Elemente. Dies sind kleine standardisierte geometrische Objekte, auf denen die physikalischen Modellgleichungen gelöst werden. Mehr hierzu in späteren Kapiteln.
Die Diskretisierung wird über sogenannte *Preprozessoren* generiert. In modernen FE Softwarepaketen sind diese Programmeinheiten integriert. Für gesonderte Diskretisierungsanforderungen gibt es jedoch auch Spezialsofware wie:
 - [Hypermesh](https://altair.com/hypermesh/)
 - [Ansa](https://www.beta-cae.com/)
 - [Gmsh](https://gmsh.info/) 

um nur einige zu nennen.
Zur Diskretisierung gehört nicht nur die Unterteilung des Gebietes in Finite Elemente. Auch die Belastungen müssen aus dem mechanischen Modell diskretisiert werden.

Das erstellte Finite-Elemente-Modell dient als Grundlage für die weiterführenden Berechnungen mit der gewählten FEM-Software - dem *Prozessor*. Innerhalb des Programms wird aus den Eingabedaten ein, in der Regel nichlineares, Gleichungssystem generiert und gelöst, das schließlich Aufschluss über relevante Größen wie Verschiebungen, Dehnungen, Spannungen, Wärmefluss, Temperaturverteilung gibt. Da die Menge an Ergebnisdaten bei großen Problemen erheblich sein kann, sind *Postprozessoren* unverzichtbar, um eine übersichtliche und grafische Auswertung zu gewährleisten. Auch diese sind in kommerziellen FE-Systemen integriert und auch hier haben sich für bestimmte Anwendungsfälle spezialisierte Softwarelösungen etabliert:
  - [Paraview](https://www.paraview.org/)
  - [Tecplot](https://tecplot.com/)
  - [Ansa](https://www.beta-cae.com/)
  - [Animator4](https://www.gns-mbh.com/products/animator4/) 

um auch hier nur einige zu nennen.

Der wichtigste Schritt für den/die Entwicklungsingenieur/in bei der FE-Simulation ist die anschließende Auswertung und Bewertung der Ergebnisse. Man sollte den Resultaten stets mit einem gewissen Maß an Skepsis begegnen und entsprechende Kontrollen hinsichtlich Plausibilität und Größenordnung durchführen. Dies kann durch Vergleiche mit Einfachmodellen und experimentellen Untersuchungen unterstützt werden.

Letztlich muss der/die Anwender/in die Resultate im Kontext der technischen Aufgabe interpretieren. Sollten die Ergebnisse nicht zufriedenstellend sein, kann es notwendig sein, die Rechnung zu wiederholen. Dies kann Änderungen im Finite-Elemente-Modell, der Idealisierung der Struktur oder der Festlegung der Belastungen nach sich ziehen. Nur durch diese iterative Vorgehensweise lässt sich ein genaues und verlässliches Bild des untersuchten technischen Systems gewinnen.

<!-- - **Nutzung von Finite-Elemente-Programmen**
  - Anwendung zur Untersuchung realer Tragwerke unter technischen Aufgabenstellungen
  - Beispiele für reale Strukturen: Pkw-Karosserie, Stockwerkrahmen
  - Beispiele für technische Aufgaben: Simulation eines Crash-Vorgangs, Ermittlung der Traglast

- **Erstellung des mechanischen Modells**
  - Idealisierte Darstellung der Struktur entscheidend
  - Weglassen von unwichtigen Strukturdetails
  - Festlegung der relevanten Belastungen

- **Diskretisierung in finite Elemente**
  - Manuelle Diskretisierung möglich bei kleinen Strukturen
  - Rechnerunterstützung erforderlich bei komplexen Strukturen wie Fahrzeugkarosserien oder Flugzeugen
  - Nutzung von Preprozessoren zur Modellerzeugung und grafischen Kontrolle

- **Finite-Elemente-Modell als Basis für Berechnungen**
  - Bereitstellung von Eingabedaten für FEM-Programme
  - Internes Generieren und Lösen von Gleichungssystemen
  - Bestimmung relevanter Größen wie Verschiebungen und Schnittkräfte
  - Verwendung von Postprozessoren zur übersichtlichen Darstellung der Ergebnisse

- **Kritische Bewertung der FEM-Ergebnisse**
  - Notwendigkeit des Misstrauens gegenüber FEM-Ergebnissen
  - Durchführung von Kontrollen für Plausibilität und Größenordnung der Ergebnisse
  - Vergleich mit Einfachmodellen und experimentellen Untersuchungen

- **Interpretation und Anpassung**
  - Interpretation der Ergebnisse im Kontext der technischen Aufgabe
  - Mögliche Wiederholung der Rechnung bei nicht zufriedenstellenden Ergebnissen
  - Anpassung des Finite-Elemente-Modells, Idealisierung der Struktur oder Belastungsfestlegung ggf. notwendig -->