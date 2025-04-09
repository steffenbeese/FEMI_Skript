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

## Der Systembegriff

> Ein System besteht aus einer Menge von Elementen (Teilsystemen), die Eigenschaften besitzen und durch Beziehungen miteinander verknüpft sind. Das System wird durch eine Systemgrenze von der Umgebung abgegrenzt und steht mit der Umgebung durch Ein- und Ausgangsgrößen in Beziehung. Die Funktion eines Systems kann durch den Unterschied der dem Zweck entsprechenden Ein- und Ausgangsgrößen beschrieben werden. Die Systemelemente können selbst wiederum Systeme sein, die aus Elementen und Beziehungen bestehen. {cite}`ehrlenspiel2013integrierte`

Die Definition eines Systems umfasst mehrere wesentliche Schritte:

1. **Identifikation der Systemgrenzen**: Hier wird festgelegt, was zum System gehört und was nicht. Diese Abgrenzung ist entscheidend, um den Fokus der Analyse zu bestimmen und irrelevante Einflüsse auszuschließen.

2. **Bestimmung der Systemelemente**: In diesem Schritt werden die Komponenten identifiziert, die Teil des Systems sind. Diese Komponenten können physikalische Objekte, biologische Organismen, soziale Gruppen,  technische Bauteile, etc. sein.

3. **Beschreibung der Beziehungen zwischen den Systemelementen**: Hier wird untersucht, wie die einzelnen Komponenten miteinander interagieren. 

Ein gut definiertes System ist entscheidend für die Genauigkeit und Zuverlässigkeit der Simulationsergebnisse. Nur wenn alle relevanten Komponenten und deren Interaktionen korrekt erfasst und modelliert werden, können valide Aussagen über das Systemverhalten getroffen werden.

**Zustand, Zustandsgröße, Zustandsvariable**

In der numerischen Simulation wird der Zustand eines Systems durch eine Reihe von Zustandsgrößen oder Zustandsvariablen beschrieben. Diese Variablen repräsentieren die Eigenschaften des Systems zu einem bestimmten Zeitpunkt.

- **Zustand**: Der Zustand eines Systems ist eine vollständige Beschreibung des Systems zu einem bestimmten Zeitpunkt. Er umfasst alle relevanten Informationen, die notwendig sind, um das Verhalten des Systems zu verstehen und vorherzusagen. Der Zustand ist somit eine Momentaufnahme des Systems, die alle wesentlichen Eigenschaften und Charakteristika beinhaltet.

- **Zustandsgröße / Zustandsvariable**: Eine Zustandsgröße ist eine messbare Eigenschaft des Systems, die den Zustand des Systems beschreibt. Beispiele für Zustandsgrößen sind Temperatur, Druck, Spannung und Verformung. Diese Größen sind oft physikalische Messwerte, die direkt beobachtet oder gemessen werden können. Es handelt sich bei Zustandsvariablen zudem meist um Felder, dass heißt sie haben eine räumliche und zeitliche Abhängigkeit.

- **Systemverhalten**: Das Systemverhalten beschreibt, wie sich das System im Laufe der Zeit entwickelt. Es umfasst die Dynamik und die Reaktionen des Systems auf äußere Einflüsse und interne Prozesse. Das Systemverhalten ist somit die zeitliche Entwicklung des Zustands des Systems und kann durch die zeitliche Abfolge von Punkten im Zustandsraum beschrieben werden.


## Klassifikation von Systemen

System können nach verschiedenen Ordnungskriterien klassifiziert werden. In der nachfolgenden Tabelle sind die wichtigsten Kriterien und deren Beschreibung aufgeführt.

| **Klassifikation** | **Beschreibung** |
|----------------|--------------|
| **Statisch** | Systeme, deren Verhalten unabhängig von der Zeit ist; Zustände ändern sich nicht. |
| **Quasistatisch** | Systeme, die sich langsam ändern, sodass sie zu jedem Zeitpunkt als statisch betrachtet werden können. |
| **Dynamisch** | Systeme, deren Verhalten sich mit der Zeit ändert; Zustände entwickeln sich über die Zeit. |
| **Zeitkontinuierlich** | Systeme, die zu jedem Zeitpunkt einen definierten Zustand haben; Änderungen erfolgen kontinuierlich. |
| **Zeitdiskret** | Systeme, die nur zu bestimmten Zeitpunkten Zustandsänderungen aufweisen; Änderungen erfolgen in diskreten Schritten. |
| **Ereignisorientiert** | Systeme, deren Verhalten durch bestimmte Ereignisse ausgelöst wird; Änderungen treten in Reaktion auf Ereignisse auf. |
| **Deterministisch** | Systeme, deren Verhalten vollständig durch Anfangsbedingungen und Regeln bestimmt ist; keine Zufälligkeit. |
| **Stochastisch** | Systeme, deren Verhalten durch Zufallsprozesse beeinflusst wird; es gibt eine gewisse Unsicherheit im Verhalten. |

```{figure} images/Systemverhalten.png
---
height: 300px
name: systemverhalten
---
Klassifikation von Systemen nach ihrem Verhalten
```

## Der Modellbegriff

In der Entwicklung mechatronischer Systeme spielen die VDI-Richtlinien 2206 und 2211 eine zentrale Rolle. Diese Richtlinien bieten eine methodische Grundlage für die Modellbildung, die ein physikalisch-mathematisches Abbild eines technischen Bauelements, einer Baugruppe oder eines komplexen Systems darstellt.

**Modellbildung und ihre Bedeutung**

Modelle sind materielle oder immaterielle Gebilde, die geschaffen werden, um für einen bestimmten Zweck ein Original zu repräsentieren. Sie können als Abbildungen oder Nachbildungen von Originalen betrachtet werden. Die Modellbildung beinhaltet die Darstellung eines physikalisch-mathematischen Modells eines vorhandenen Systems oder eines zu entwickelnden Systems. Der Zweck der Modellbildung besteht darin, das Original durch das Modell zu ersetzen und es als Stellvertreter des Originals zu nutzen, um Rückschlüsse auf das Original zu ziehen.

Modelle können somit als zweckgerichtete, vereinfachte Abbildungen oder Nachbildungen von Originalen aufgefasst werden. Sie umfassen eine Vielzahl von Konstrukten, darunter Anschauungsmodelle, Prototypen, Konstruktionszeichnungen, Schaltpläne, mathematische Gleichungen, aber auch Gedankenmodelle bzw. mentale Modelle, Vorstellungen und Bilder.

**Anforderungen an Modelle**

Modelle müssen original- und realitätsnah sein, um charakteristische Eigenschaften und Verhalten des Originals genau zu beschreiben. Der Modellzweck hängt von den Lebensphasen des Produkts ab, und es ist wichtig, ein angemessenes Aufwand-/Nutzen-Verhältnis zu gewährleisten. Der Aufwand für Modellierung und Analyse ist eng mit dem Detaillierungsgrad (Granularität) des Modells verbunden. Eine sehr genaue Modellierung ist nicht immer notwendig; Unsicherheiten können den Nutzen eines detaillierten Modells in Frage stellen.

Das Modellverhalten muss im Gültigkeitsbereich dem realen Systemverhalten entsprechen, um die Modellgültigkeit zu gewährleisten. Das Verhalten resultiert aus den Eigenschaften der Modellelemente und deren Verknüpfungen. Bei mehreren geeigneten Modellierungsansätzen sollte die einfachste Methode bevorzugt werden, um die Modelleffizienz zu maximieren. Es gibt keine allgemeinen Regeln für die Herleitung eines einfachen, effizienten und gültigen Modells; vielmehr sind Erfahrung und Vorwissen entscheidend.

## Modellbildung

```{admonition} Ziel der Modellbildung
:class: tip
Modellbildung ist die Schaffung eines Modells, das dem Untersuchungszweck entspricht und demgemäß verändert und ausgewertet werden kann, um damit Rückschlüsse auf das Original ziehen zu können.
```

```{figure} images/Problemloesung.png
---
height: 300px
name: problemlösung
---
Problemlösung im Ingenieurwesen nach {cite}`vajna2009cax`
```

Die Modellabstraktion ist ein zentraler Bestandteil der Ingenieurwissenschaft, da sie die Realität einer Berechnung zugänglich macht. Durch die Abstraktion werden irrelevante Details vernachlässigt, während die relevanten Details erhalten bleiben. Das Ziel ist es, ein Modellergebnis zu erzielen, das eine hohe Relevanz für die Lösung der realen Problemstellung aufweist.

**Modellabstraktion als Grundlage für Analyse und Design**

Ein Modell dient als Grundlage für die Analyse und das Design von Systemen. Durch die Konzentration auf die spezifischsten und wichtigsten Merkmale und die Abstraktion von unwichtigen Eigenschaften und Details wird die Komplexität der Problemstellung reduziert. Diese Vereinfachung ist bereits ein Ziel der Analyse, da sie die Handhabbarkeit der Problemstellung ermöglicht. Ohne eine solche Vereinfachung wären viele Problemstellungen innerhalb der praktischen Beschränkungen von Zeit und Ressourcen äußerst komplex zu simulieren oder nicht effektiv zu analysieren.

**Vorteile einfacherer Modelle**

Einfachere Modelle sind von Natur aus leichter zu entwickeln, zu verstehen und zu modifizieren. Dies ist besonders wertvoll in iterativen Designprozessen, in denen häufige Aktualisierungen von Modellen erforderlich sind. Abstrakte Modelle können als gemeinsame Sprache für Ingenieure aus verschiedenen Disziplinen oder mit unterschiedlichem Fachwissen dienen. Sie reduzieren Missverständnisse und ermöglichen eine effektivere Zusammenarbeit in Ingenieurprojekten.

Darüber hinaus erfordern einfachere Modelle weniger Rechenleistung und Zeit für die Lösung. Dies ermöglicht mehr Iterationen, Sensitivitätsanalysen und die Untersuchung von Designalternativen innerhalb angemessener Zeitrahmen. Die Vereinfachung durch Modellabstraktion trägt somit wesentlich zur Effizienz und Effektivität des gesamten Design- und Analyseprozesses bei.

```{figure} images/Modellbildung.png
---
height: 400px
name: modellbildung
---
Modellbildungsprozess nach {cite}`vajna2009cax`
```
In Abbildung {numref}`modellbildung` ist der Modellbildungsprozess dargestellt. Die Modellplanung beginnt mit einer IST-Analyse, in der die Aufgabenstellung zu klären und zu präzisieren ist. Dabei treten typischerweise Fragen auf wie ({cite}`vajna2009cax`):

- Was ist das zu untersuchende Original (System, Elemente, Systemgrenzen)? 
- Welche Fragestellungen sollen behandelt werden (Festlegung des Modellzwecks)? 
- Welche Sichtweisen auf das zu untersuchende Original (Systemaspekte, Bewertungskriterien) sind für den Modellzweck notwendig?
- Welche Eigenschaften müssen für die gewünschte Bewertung (z. B. zur Eigenschaftsabsicherung) herangezogen werden? 
- Welche Effekte (Details) müssen daher berücksichtigt oder können vernachlässigt werden? 
- Welche Testsituationen sind zu untersuchen („Lastfälle“, Testszenarien, „use cases“)? 
- Welche Parameter bzw. Zustandsgrößen eines mathematischen Modells werden als vorgegeben (Parameter), welche als Zustandsvariablen betrachtet?
- Welche Ergebnisse sind zur Klärung der Fragestellungen erforderlich und in welcher Form sollen die Ergebnisdaten aufbereitet und dokumentiert werden (Ergebnisdarstellung und Dokumentation)? 
- Welche Relevanz und Signifikanz haben die zu erwartenden Ergebnisse in Bezug auf die Fragestellungen? Wird die Fragestellung durch die Ergebnisse auch wirklich beantwortet?

**Verifikation und Validierung**

Jedes Modell stellt lediglich eine mehr oder weniger genaue Annäherung an das Original (z.B. ein reales System) dar. Daher ist es nach der Modellentwicklung notwendig zu überprüfen, ob das Modell mit seinen Idealisierungen das zu untersuchende Original ausreichend genau abbildet. Die Verifikation untersucht, ob sich das Modell grundsätzlich plausibel verhält, und bezieht sich dabei auf das Modellverhalten, unabhängig von Vergleichen mit einem konkreten Original. Die Modell-Verifikation betrifft somit die Überprüfung der Plausibilität des Modellverhaltens an sich, also für „fiktive“ Originale. Die Validierung hingegen liefert eine Aussage darüber, ob das erstellte Modell konkrete Originale hinreichend beschreibt und in welchem Bereich das Modell gültig ist (Grenzen des Modells).

## Ansätze zur Abstraktion mechanischer Modelle


Zur Abstraktion mechanischer Modelle können folgende Ansätze angewendet werden:

1. Vereinfachung der Geometrie
   - Abstraktion durch einfachere geometrische Grundkörper
   - Komplexer Maschinenrahmen als Skelettstruktur aus Balken oder ein dünnwandiges Druckgefäß als Schale modelliert werden

2. Dimensionsreduktion
   - Übergang von einer 3D-Darstellung zu einer 2D- (ebener Spannungszustand, ebener Dehnungszustand, rotationssymmetrisch) oder sogar 1D-Darstellung (Balken, Stäbe)

3. Vereinfachung der Materialeigenschaften
   - Wenn angemessen, linear-elastische Materialmodelle anstelle komplexerer nichtlinearer oder anisotroper Modelle

4. Abstraktion der Randbedingungen
   - Darstellung komplexer Lagerungen oder Lasten durch idealisierte Randbedingungen
   - Z. B. feste, gelenkige oder rollende Lagerungen, oder durch die Verwendung von Punktlasten oder verteilten Lasten

5. Systemebenenabstraktion
   - Subsysteme oder Komponenten werden als Black Boxes mit definierten Ein- und Ausgängen modelliert
   - Schwerpunkt auf ihrem Gesamtverhalten und nicht auf komplizierten internen Details
   - Ansatz besonders nützlich für die Analyse großer, komplexer Systeme, indem diese in kleinere, besser handhabbare Teile zerlegt werden

6. Vereinfachte physikalische Bilanzgleichung