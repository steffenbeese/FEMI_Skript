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

# Fehlerquellen

Fehler in FEM-Modellen entstehen selten nur an einer Stelle. In der Praxis wirken Modellannahmen, Vernetzung, Solver-Einstellungen und Auswertung zusammen. Die folgenden Punkte basieren auf den Abschlussfolien der Vorlesung zur Vernetzung und fassen typische Stolperstellen systematisch zusammen.

## Typische Fehler nach Prozessschritt

| Prozessschritt | Anwenderseitig | Programm-Handhabung |
| --- | --- | --- |
| Pre-Processing | falsche Idealisierung, ungeeignete Elemente, verletzte Kompatibilität, ungenaue Materialdaten | schlechte Vernetzung, unpassende Elementformulierung, fehlender Elementcheck |
| Lösungsverfahren | fehlerhafte Randbedingungen, ungeeignetes Lösungsverfahren, zu grobes Konvergenzkriterium | zu ungenauer Gleichungslöser, ungenaue Rückrechnung, ignorierte Warnungen |
| Post-Processing | falsche Selektion von Ergebnissen | falsche Mittelung, irreführende Darstellung |

## Häufige Fehlerbilder und deren Bedeutung

### 1) Starrkörperverschiebungen

Wenn einem Modell wesentliche Lagerungen oder Kontaktbindungen fehlen, besitzt das System freie Starrkörpermoden.

Typische Anzeichen:

- Abbruch der Analyse ohne physikalisch sinnvolle Lösung,
- schlechte oder ausbleibende Konvergenz,
- Meldungen wie Zero Pivot (fehlende Steifigkeit in der Hauptdiagonale der Steifigkeitsmatrix).

Praktische Gegenmaßnahmen:

- Randbedingungen auf Vollständigkeit prüfen,
- Kontaktdefinitionen kontrollieren (offene Kontakte, falsche Normalenrichtung, Aktivierungszustand),
- bei linearen statischen Tests einen schnellen Lagerungs-Check mit vereinfachtem Modell durchführen.

### 2) Überrechnung von Stabilitätspunkten

Warnungen zu negativen Eigenwerten werden oft vorschnell ignoriert. Das ist riskant: Negative Eigenwerte können auf Instabilität, aber auch auf numerische Kontaktartefakte hinweisen.

Praktische Gegenmaßnahmen:

- Warnungen immer interpretieren statt ausblenden,
- Lastpfad und Inkrementierung prüfen,
- Kontakt- und Geometrienichtlinearitäten getrennt testen,
- bei Bedarf eine lineare Beul- oder Eigenwertanalyse als Zusatzcheck durchführen.

### 3) „Starrkörpersimulation“ über unrealistisch hohe Steifigkeiten

Eine häufige Abkürzung ist das künstliche Erhöhen von Steifigkeiten, um Bereiche „quasi starr“ zu machen. Dadurch entstehen schlecht konditionierte Gleichungssysteme und numerische Fehler (kleine Differenzen sehr großer Zahlen).

Praktische Gegenmaßnahmen:

- wenn möglich echte Starrkörper- oder Rigid-Body-Optionen verwenden,
- Materialkennwerte nicht um Größenordnungen verfälschen,
- Konditionsprobleme über Solver-Logs und Reaktionskräfte plausibilisieren.

### 4) Entartete Elemente

Entartete oder stark verzerrte Elemente sind besonders für Spannungs- und Dehnungsauswertungen kritisch.

Typische Ursachen:

- schlechte Elementabmessungen (hohe Aspect Ratio),
- ungünstige Innenwinkel,
- lokale Selbstüberschneidungen oder nahezu kollabierte Elemente.

Praktische Gegenmaßnahmen:

- Elementqualitätsmetriken vor dem Lösen prüfen,
- Netz lokal verfeinern und Übergänge glätten,
- Elementtyp an Lastfall anpassen (z. B. bei Biegung nicht zu grob mit linearen Elementen arbeiten).

## Checkliste vor dem Rechnen

1. Ist das Modell kinematisch korrekt gelagert?
2. Sind Materialdaten, Einheiten und Randbedingungen konsistent?
3. Erfüllt das Netz die Qualitätskriterien (Winkel, Aspect Ratio, Jacobian-bezogene Kennwerte)?
4. Passen Elementtyp und Netzfeinheit zur Zielgröße (z. B. Spannung, Verformung, Eigenfrequenz)?
5. Werden Solver-Warnungen dokumentiert und fachlich bewertet?
6. Ist die Ergebnisdarstellung plausibel (Mittelung, Schnitte, relevante Hotspots)?

## Merksatz

Die meisten FEM-Fehler sind nicht reine Solverfehler, sondern Modellierungsfehler mit numerischen Symptomen. Gute Ergebnisse entstehen durch sauberes Pre-Processing, robuste Lösungseinstellungen und kritisches Post-Processing.

```{admonition} Fragen zum Kapitel
:class: warning

1. Welche drei Prozessschritte werden zur Einordnung von FEM-Fehlern unterschieden?
2. **Welche typischen Anzeichen deuten auf Starrkörperverschiebungen im Modell hin?**
3. Warum dürfen Warnungen zu negativen Eigenwerten nicht einfach ignoriert werden?
4. Was passiert, wenn Steifigkeiten künstlich unrealistisch hoch gesetzt werden?
5. Nennen Sie zwei typische Ursachen für entartete Elemente und zwei Gegenmaßnahmen dagegen.
```


