---
jupytext:
  formats: md:myst
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

+++ {"editable": true, "slideshow": {"slide_type": ""}}

# Zeitabhängige Problemstellungen in der FEM

Bisher haben wir mit der FEM statische oder quasistatische Problemstellungen gelöst. Wenn die zeitliche Änderung der Belastung jedoch schnell erfolgt und Trägheitseffekte nicht mehr vernachlässigt werden können, dann müssen wir dies auch in der Simulation abbilden können. Wir sprechen dann von einer transienten FEM-Analyse. Zur Abgrenzung sei erwähnt, dass nicht jedes zeitabhängige Problem eine transiente Analyse erfordert. Sind zum Beispiel die Lasten zeitlich veränderlich, jedoch die Änderungsgeschwindigkeit nur gering, oder liegt lediglich ein zeitabhängiges Materialverhalten vor, so kann weiterhin quasistaisch gerechnet werden.

Transiente zeitabhängige Rechnungen müssen durchgeführt werden für:

- Zeitabhängige Belastungen mit hohen Änderungsraten:

    - z. B. stoßartige oder pulsierende Kräfte, Erdbebenlasten, Windböen, Druckwellen.
      
- Trägheitseffekte (Masse) sind nicht vernachlässigbar:

    - Bei schnellen Bewegungen oder bei schlagartigen Beanspruchungen.

- Wenn Dämpfung berücksichtigt werden muss:

    - z. B. bei schwingenden Strukturen mit Energieverlust durch Reibung oder Materialdämpfung.



<!-- <iframe width="640" height="480" src="https://youtu.be/Jq9qCcijCAU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->


Unterliegt ein System Schwingungen oder harmonischen Belastungen wird für gewöhnlich auf eine transiente Rechnung verzichtet und stattdessen das Schwingungsverhalten im Frequenzbereich untersucht (Harmonische Analyse).

Typische Anwendungsfälle für transiente Berechnungen sind:

| Anwendungsfall                          | Beschreibung                                                                    |
| --------------------------------------- | ------------------------------------------------------------------------------- |
|  **Erdbebenanalyse von Bauwerken**    | Zeitverlauf der Bodenbeschleunigung wird als Eingabe verwendet.                 |
|  **Crash-Simulationen (Automobil)**   | Simulation des Aufpralls mit hoher Geschwindigkeit und kurzer Dauer.            |
|  **Schlag- und Stoßvorgänge**         | z. B. Hammerschlag, Werkzeugkontakt, Fallversuche.                              |
|  **Falltests von Verpackungen**       | Analyse, wie ein Produkt nach dem Fall auf den Boden reagiert.                  |
| **Zugdurchfahrten auf Brücken**      | Dynamische Beanspruchung durch zeitlich veränderliche Belastung.                |
| **Raumfahrt / Luftfahrtkomponenten** | Analyse der Reaktion auf plötzliche Lasten (Start, Abtrennung, Wiedereintritt). |


```{figure} images/LS-DYNA_Crash.png
---
name: LS-DynaCrash
alt: LS Dyna Crash Simulation
width: 500px
---
Simulation einer LKW-Unterfahrung mit LS-Dyna [link](https://youtu.be/Jq9qCcijCAU)
```


<!-- ```{figure} images/Bullet_Penetration_LS_DYNA.png
---
name: LS-Bullet
alt: LS Dyna Bullet Penetration
width: 500px
---
Simulation einer Patrone durch eine Stahlwand mit LS-Dyna [link](https://youtu.be/VZopWkFPnxA)
``` -->

Grundlage der stukturmechanischen transienten Berechnungen ist die Impulsbilanz:

```{math}
:label: Impulsbilanz_dynamisch
\varrho \ddot{\bm{u}} =\Div{\bm{\sigma}} + \rho \bm{b} 
```

Hierbei stellt der Term auf der linken Seite die Trägheitskräfte dar. Durch die Bildung der schwachen Form und anschließende Diskretisierung erhält man im FEM-Kontext folgendes Differentialgleichungssystem:

```{math}
:label: FEM_dynamisch
\bm{M} \ddot{\bm{u}} + \bm{C} \dot{\bm{u}} + \bm{K} \bm{u} = \bm{f}(t)
```

mit:
- $\bm{M}$ - Massenmatrix
- $\bm{C}$ - Dämpfungsmatrix
- $\bm{K}$ - Steifigkeitsmatrix
- $\bm{f}(t)$ - äußere Lasten



Ein weiterer wichtiger Anwendungsfall transienter Berechnungen sind instationäre Wärmeleitungsprobleme, Stofftransportprobleme und Simulationen instationärer elektro-magnetischer Felder.


```{admonition} Bild von Wärmeleitung
:class: warning

muss noch erstellt werden.

```

```{code-cell} ipython3

```
