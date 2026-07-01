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

# Wärmeleitung

Bei der Wärmeleitung unterscheiden wir zwischen instationären und stationären Problemstellungen. Stationäre Problemstellungen sind solche, bei denen sich die Temperaturverteilung im Körper nicht mehr ändert. Instationäre Problemstellungen sind solche, bei denen sich die Temperaturverteilung im Körper noch ändert - also zum Beispiel Aufheiz- oder Abkühlvorgänge. Die zugrundeliegende Physikalische Bilanzgleichung ist der erste Hauptsatz der Thermodynamik. Für ein instationäres Wärmeleitungsproblem fester Körper lautet die Gleichung:

```{math}
:label: eq_energiebilanz
\begin{equation}
\varrho c_p \frac{\partial T}{\partial t} = \nabla \cdot (k \nabla T) + r \; .
\end{equation}
```

In dieser Gleichung wurde für den Wärmestrom $\bm{q}$ bereits die _Fourie'sche_ Wärmeleitung mit dem Wärmleitkoeffizienten $k$ verwendet. Weiterhin beschreibt $c_p$ in Gleichung {eq}`eq_energiebilanz` die spezifische Wärmekapazität und $\varrho$ die Dichte des Materials. Der Term $r$ beschreibt die Wärmezufuhr in Form eines Quellterms. Als Randbedingungen  können die Temperaturen (_Dirichlet_) direkt auf den Knoten vorgeschrieben werden oder der Wärmefluss über den Rand. Auch Konvektionsströme können über Randbedingungen  approximiert werden. Hierbei ist aber der genaue Zahlenwert der Randbedingung oft schwer zu bestimmen. Eine gute Quelle für typische Konvektionskennwerte stellt der VDI Wärmeatlas {cite}`vdiWaerme` dar. Für genauere Betrachtungen ist es jedoch oft erforderlich eine Strömungssimulation zu betreiben und die Konvektionskennwerte aus dieser zu extrahieren.

Für stationäre Wärmeleitung vereinfacht sich Gleichung {eq}`eq_energiebilanz` zu:

```{math}
:label: eq_energiebilanz_stationaer
\begin{equation}
0 = \nabla \cdot (k \nabla T) + r \; .
\end{equation}
```

Nach einer erfolgreichen Temperaturberechnung kann man eine strukturmechanische Berechnung durchführen um den thermischen Verzug und die thermischen Spannungen zu ermitteln.

## Zeitintegration für thermisch instationäre Wärmeleitung

Da der Prozess der Wärmeleitung für gewöhnlich recht langsam abläuft wird die Gleichung {eq}`eq_energiebilanz` für gewöhnlich mit einer impliziten Zeitintegration gelöst. Gleichung {eq}`eq_energiebilanz` hat genau die Form:

\begin{equation*}
 \frac{dy}{dt} = f(y, t) \; ,
\end{equation*}

sodass die Methoden der Zeitintegration aus Kapitel [Transiente Dynamik](TransienteDynamik.md#implizite-zeitintegration) direkt angewendet werden können. Oftmals wird das $\theta$-Verfahren mit $\theta = 0.5$ verwendet, da es eine gute Balance zwischen Stabilität und Genauigkeit bietet.
