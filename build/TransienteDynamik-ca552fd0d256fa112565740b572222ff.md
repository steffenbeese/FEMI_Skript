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

## Transiente Dynamik

Soll die Bewegungsgleichung {eq}`FEM_dynamisch`:

\begin{equation*}
\bm{M} \ddot{\bm{u}} + \bm{C} \dot{\bm{u}} + \bm{K} \bm{u} = \bm{f}(t) \; .
\end{equation*}

im Zeitbereich gelöst werden muss das Differentialgleichungssystem integriert werden. Dies ist ein numerisches Problem, da die Lösung nicht analytisch bestimmt werden kann. Zur Lösung des Problems gibt es verschiedene Verfahren, die sich in der Genauigkeit und Stabilität unterscheiden. Prinzipiell unterscheiden wir zwischen expliziten und impliziten Verfahren. Für eine Einführung in die Methoden der Zeitintegration betrachten wir zunächst wieder einen einfachen Feder-Masse-Schwinger.

### 1D Feder-Masse-Schwinger

Im folgenden wird die Bewegung einer Masse $m$ an einer Feder mit der Federsteifigkeit $k$ und dem Dämpfungsbeiwert $c$ betrachtet: 

```{figure} images/spring.png
---
name: 1D-Feder_Masse
alt: Gedämpfter Feder-Masse-Schwinger
width: 500px
---
Gedämpfter Feder-Masse-Schwinger
```

Bei einer Kraftanregung nach der Funktion:

\begin{equation*}
A\cdot\cos{\omega_F t}
\end{equation*}

ergibt sich folgende analytische Lösung für die Auslenkung $x(t)$:

```{figure} images/FederMasseSchwinger_analytisch.png
---
name: Loesung_FederMasseSchwinger_analytisch
alt: Analytische Lösung des Feder Masse Systems
width: 500px
---
Analytische Lösung des Feder-Masse-Schwingers
```

| Parameter | Wert |
| --------- | ---- |
| $m$       | 5 kg |
| $k$       | 10 N/m |
| $c$       | 0.5 Ns/m |
| $A$       | 3 N |
| $\omega_F$ | 2 rad/s |
| $x_0$     | 0.8 m |
| $v_0$     | 0.0 m/s|

Zur Bestimmung der Verschiebung der Masse $x(t)$ zu einem beliebigen Zeitpunkt muss die Differentialgleichung des Feder-Masse-Schwingers:

\begin{equation*}
 m \ddot{x} + c \dot{x} + k x = A \cos(\omega_F t)
\end{equation*}

 zeitlich integriert werden. Dies ist eine Differentialgleichung zweiter Ordnung. Die meisten numerischen Verfahren sind jedoch für Systeme erster Ordnung entwickelt worden. Daher wird die Differentialgleichung in ein System erster Ordnung umgewandelt:

 \begin{equation*}
 \begin{pmatrix}
  \dot{u} (t) \\
  \dot{v}
 \end{pmatrix} = \begin{pmatrix} v(t) \\ -\frac{c}{m} v(t) - \frac{k}{m} x(t) + A \cos(\omega_F t) \end{pmatrix}
 \end{equation*}

 Allgemein lässt sich dann schreiben:

\begin{equation}
\dot{y}(t) = f(y, t), \quad y(t_0) = y_0
\end{equation}

Gesucht ist eine numerische Approximation der Lösung $ y(t) $ auf einem Zeitintervall $[t_0, T]$.


### Explizite Methoden der Zeitintegration 

Bei der expliziten Zeitintegration wird die Lösung $ y(t_{n+1}) $ **nur** aus den bekannten Werten $y(t_n)$ und $\dot{y}(t)$ berechnet. Sie haben eine bedingte Stabilität, was bedeutet, dass die Schrittweite der Integration klein genug sein muss, um numerische Stabilität zu gewährleisten. Ein Vorteil dieser Methoden ist die einfache Implementierbarkeit. Da keine komplexen Gleichungssysteme gelöst werden müssen, ist die Berechnung pro Zeitschritt schneller. Bei einer großen Anzahl von Integrationsschritten können Rundungsfehler akkumulieren. Die explizite Zeitintegration findet Anwendung in verschiedenen Bereichen, wie zum Beispiel:

- **Strukturdynamik:** In der Erdbebeningenieurwissenschaft und beim Entwurf von Gebäuden unter dynamischer Last.

- **Crash-Simulation:** In der Automobilindustrie zur Simulation von Kollisionen.

- **Ballistik:** Zur Berechnung der Flugbahnen von Projektilen.

- **Aerodynamik:** Bei der Simulation von transienten Strömungen um Objekte.

#### Explizites Eulerverfahren

Das wohl einfachste Verfahren zur expliziten Zeitintegration ist das explizite Eulerverfahren. Es ist ein einstufiges Verfahren und wird auch als Forward Euler-Verfahren bezeichnet. Die Lösung wird wie folgt berechnet:

1. **Diskretisierung der Zeit**:
   - Wähle eine Schrittweite $ \Delta t > 0 $ und diskretisiere das Intervall:
     $ t_n = t_0 + n \Delta t $ für $ n = 0, 1, 2, \dots, N $, wobei $ N \Delta t = T - t_0 $.

2. **Iterative Berechnung**:
   - Starte mit dem Anfangswert $ y_0 $ zum Zeitpunkt $ t_0 $.
   - Approximiere die Lösung an den diskreten Zeitpunkten $ t_n $ durch:
     \begin{equation*}
     y_{n+1} = y_n + \Delta t \cdot f(y_n, t_n)
     \end{equation*}
   - Dabei ist $ y_n $ die Approximation von $ y(t_n) $.

Das Verfahren hat eine **Fehlerordnung** von $ O(\Delta t) $. Es
ist konditional stabil – zu große Schrittweiten $ \Delta t $ können zu numerischer Instabilität führen (insbesondere bei **steifen** DGLs).


#### Methode von Heun

Das Verfahren von Heun ist ebenfalls ein Einschrittverfahren. Es ist das einfachste Verfahren der **Runge-Kutta-Familie**. Die Lösung wird wie folgt berechnet:

1. **Diskretisierung der Zeit**:
   - Wähle eine Schrittweite $ \Delta t > 0 $ und diskretisiere das Intervall:
     $ t_n = t_0 + n \Delta t $ für $ n = 0, 1, 2, \dots, N $, wobei $ N \Delta t = T - t_0 $.

2. **Iterative Berechnung**:
   - Starte mit dem Anfangswert $ y_0 $ zum Zeitpunkt $ t_0 $.
   - Approximiere die Lösung an den diskreten Zeitpunkten $ t_n $ durch einen zweistufigen Prozess:
     1. Berechne zunächst einen vorläufigen Wert $ \tilde{y}_{n+1} $ mittels Euler-Vorwärtsverfahren:
        \begin{equation*}
        \tilde{y}_{n+1} = y_n + \Delta t \cdot f(y_n, t_n)
        \end{equation*}
     2. Nutze diesen vorläufigen Wert, um den Schlusswert $ y_{n+1} $ zu verbessern:
        \begin{equation*}
        y_{n+1} = y_n + \frac{\Delta t}{2} \left( f(y_n, t_n) + f(\tilde{y}_{n+1}, t_{n+1}) \right)
        \end{equation*}
   - Dabei ist $ y_n $ die Approximation von $ y(t_n) $ und $ t_{n+1} = t_n + \Delta t $.


Das Heun-Verfahren hat eine **Fehlerordnung** von $ O(\Delta t^2) $, was es genauer macht als das Euler-Verfahren.
Auch das Heun-Verfahren ist nur bedingt stabil, erfordert aber aufgrund seiner höheren Genauigkeit möglicherweise weniger restriktive Schrittweiten als das Euler-Verfahren.
Es ist komplexer in der Implementierung als das Euler-Verfahren, da zwei Auswertungen der Funktion $ f $ pro Schritt erforderlich sind.

Nachfolgend ist für eine Schrittweite von $\Delta t = 0.075$ die Lösungen des Euler-Vorwärtsverfahrens und des Heun-Verfahrens dargestellt:

```{figure} images/Federschwinger_Euler.png
---
name: Loesung_FederMasseSchwinger_numerisch_Explizit
alt: Analytische und numerische Lösung des Feder Masse Systems im Vergleich
width: 500px
---
Analytische und numerische Lösung des Feder-Masse-Schwingers im Vergleich.
```

In der dargestellten Verschiebungslösung ist zu erkennen, dass die numerische Lösung des Euler-Verfahrens eine deutlich geringere Genauigkeit aufweist als die Methode nach Heun Lösung. Für die gegebenen Parameter und die Schrittweite ist das Euler Vorwärtsverfahren instabil - Die Amplituden nehmen stetig zu. 

#### Weitere Explizite Zeitintegrationen

weitere weit verbreite Verfahren der expliziten Zeitintegration sind das zentrale Differenzverfahren, das Runge-Kutta-Verfahren 4. Ordnung und das Adams-Bashforth-Verfahren. 


### Explizite Zeitintegration in der Strukturdynamik

Wenn hohe Frequenzen (Crash) oder Schockwellen die Lösung dominieren, dann sind sehr kleine Zeitschritte notwendig. Für diese Fälle ist eine explizite Zeitintegration die effizienteste Möglichkeit die Bewegungsgleichung zu integrieren. Die meist verwendete Methode stellt das zentrale Differenzverfahren dar. Die Geschwindigkeit $\bm{v}$ und die Beschleunigung $\bm{a}$ zum Zeitpunkt $t_n$ werden bestimmt zu:

```{math}
:label: eq_central_diff
\begin{align}
\bm{v}_n & = \frac{\bm{u}_{n+1} - \bm{u}_{n-1}}{2 \Delta t} \\
\bm{a}_n & = \frac{\bm{u}_{n+1} - 2 \bm{u}_n + \bm{u}_{n-1}}{\Delta t^2}\; .
\end{align}
```

Setzt man diese Approximationen in die Bewegungsgleichung ein, so erhält man:

\begin{equation*}
 \bm{M} \left( u_{n+1}-2u_n+u_{n-1}  \right) + \frac{\Delta t}{2}\bm{C} \left(u_{n+1}-u_{n-1}\right) + \Delta t^2 \bm{K} u_{n} = \Delta t^2 \bm{f}(t_n) \; .
\end{equation*}

Diese Gleichung muss man nach $u_{n+1}$ auflösen und erhält:

```{math}
:label: eq_central_diff_2
\begin{equation}
\left( \bm{M} + \frac{\Delta t}{2}\bm{C} \right) u_{n+1} = \Delta t^2 \left( \bm{f}(t_n) - \bm{K} u_n \right) + \frac{\Delta t}{2} \bm{C} u_{n-1} + \bm{M} \left( 2 u_n -  u_{n-1}\right) \; .
\end{equation}
```

Da die Massenmatrix $\bm{M}$ und die Dämpfungsmatrix $\bm{C}$ konstant sind, ergibt sich ein besonders effizientes Verfahren, wenn die beiden im Vorfeld faktorisiert werden und während der Zeitintegration nur noch die Rücksubsitution durchgeführt werden muss. Eine weitere Technik ist das sogenannte _Mass-Lumping_. Dabei werden $\bm{M}$ und $\bm{C}$ durch Diagonalmatrizen approximiert. Eine Invertierung dieser ist dann trivial und Gleichung {eq}`eq_central_diff_2` kann nur über Vektoroperationen direkt nach $u_{n+1}$ aufgelöst werden (In Grafikkarte beschleunigbar).

Explizite Zeitintegrationen sind wie bereits erwähnt **nicht unbedingt stabil**. Der gewählte Zeitschritt darf nicht größer als der kritische Zeitschritt $\Delta t\rs{crit}$ sein:

```{math}
:label: Courant
\begin{equation}
\Delta t\rs{crit} = \delta \frac{h}{c_L} \;,
\end{equation}
```

wobei $h$ die Elementgröße, $c_L$ die Schallgeschwindigkeit im Material und $\delta$ ein Reduktionsfaktor $(0.2\leq \delta \leq 0.9)$ darstellt. Die Gleichung {eq}`Courant`  wird auch als das **Courant**-Kriterium bezeichnet. Der kritische Zeitschritt richtet sich übrigens nach dem kleinsten Element im Modell. Ein einziges zu kleines Element kann somit das Gesamte Lösungsverhalten massiv stören.

Die Schallgeschwindigkeit $c_L$ in Gleichung {eq}`Courant` ist definiert als:
\begin{equation}
c_L = \frac{3K(1-\nu)}{\varrho(1+\nu)} \; .
\end{equation}
Damit kann die kritische Zeitschrittweite vergrößert werden, ind dem die Dichte lokal ebenfalls vergrößert wird. Dieses Verfahren nennt sich Massenskalierung. Man sollte darauf achten, dass nicht zu viel Masse hinzugefügt wird, da sich hierdurch das Lösungsverhalten des Systems ändert.


### Implizite Zeitintegration

Implizite Zeitintegrationsverfahren sind eine Klasse von Algorithmen, die in der numerischen Lösung von Differentialgleichungen verwendet werden. Im Gegensatz zu expliziten Methoden, bei denen der neue Zustand eines Systems ausschließlich auf Basis des aktuellen Zustands und vergangener Zustände berechnet wird, berücksichtigen implizite Verfahren auch zukünftige Werte. Dies macht sie oft stabiler und ermöglicht größere Zeitschritte, besonders bei steifen Problemen. Dafür ist es oftmals erforderlich ein nichtlineares Gleichungssystem zu lösen. Dies macht die impliziten Verfahren rechenaufwendiger als die expliziten Verfahren.


#### Euler-Rückwärts Verfahren

Das Euler-Rückwärts Verfahren, auch bekannt als implizites Euler-Verfahren, ist eine Methode zur numerischen Lösung von gewöhnlichen Differentialgleichungen (ODEs). Es wird oft verwendet, wenn ein System steife ODEs beinhaltet oder wenn eine höhere Stabilität als beim expliziten Euler-Verfahren erforderlich ist.

1. **Ausgangspunkt:**
   Gegeben sei die ODE $ \frac{dx}{dt} = f(t, x) $ mit Anfangsbedingung $ x(t_0) = x_0 $.

2. **Diskretisierung der Zeit:**
   Die Zeitachse wird in diskrete Schritte unterteilt: $ t_{n+1} = t_n + \Delta t $, wobei $ \Delta t $ die Schrittweite darstellt.

3. **Rückwärtsschritt:**
   Statt den nächsten Wert anhand des aktuellen zu berechnen, bestimmt man ihn über den unbekannten nächsten Wert:

   \begin{equation*}
   x_{n+1} = x_n + \Delta t \cdot f(t_{n+1}, x_{n+1})
   \end{equation*}

   Hierin liegt die Implizität der Methode, da $ x_{n+1} $ auf beiden Seiten der Gleichung vorkommt.

4. **Lösen der impliziten Gleichung:**
   Zur Bestimmung von $ x_{n+1} $ muss häufig eine algebraisches Gleichungssystem gelöst werden. Dies erfordert iterative Methoden wie das Newton-Verfahren. Im folgenden wird der Index $n+1$ weggelassen und einfach $x$ geschrieben. Ein neuer Index $k$ wird eingeführt, welcher die Iteration des Newton-Verfahrens kennzeichnet.

   \begin{align*}
        R(x_{k+1}) & = x_{n+1} - \left( x_n + \Delta t \cdot f(t_{n+1}, x_{n+1}) \right) & \rightarrow & 0 \\
        R(x_{k+1}) & \approx R(x_k) + \frac{\partial R(x_k)}{\partial x_k} \Delta x_k & = 0 &\\
        \Delta x_k & = -R(x_k) \left(\frac{\partial R(x_k)}{\partial x_k} \right)^{-1} & & \\
        x_{k+1} & =x_k + \Delta x_k & &
   \end{align*}

    Hierbei ist $\frac{\partial R(x_k)}{\partial x_k}$ die Jacobian $J$ des Residuums und für das implizite Eulerverfahren gleich:

    \begin{equation*}
        J = 1 - \Delta t \frac{\partial f(t_{n+1})}{\partial x_k}
    \end{equation*}

    Für steife Differentialgleichungen und Systeme mit großen Schrittweiten bietet dieses Verfahren bessere Stabilitätseigenschaften.Es ist **unbedingt stabil** (A-stabil). Egal welche Schrittweite genutzt wird, die Lösung bleibt stabil. Das Verfahren hat eine **Fehlerordnung** von $ O(\Delta t) $. Wegen der Notwendigkeit, eine implizite Gleichung zu lösen, ist der Rechenaufwand pro Schritt höher.


```{figure} images/Federschwinger_Euler_imp.png
---
name: Loesung_FederMasseSchwinger_numerisch_Implizit
alt: Analytische und numerische Lösung des Feder Masse Systems im Vergleich
width: 500px
---
Analytische und numerische Lösung des Feder-Masse-Schwingers im Vergleich.
```

In der dargestellten Verschiebungslösung ist zu erkennen, dass die numerische Lösung des impliziten Euler-Verfahrens eine bessere Stabilität als das explizite Euler-Verfahren aufweist. Die Schwingungen schaukeln sich nicht auf. Dennoch ist mit den gegebenen Parametern die Lösung sehr ungenau.

#### Die $\theta$-Methode

Die $\theta$ Methode stellt eine lineare Kombination aus explizitem und impliziten Verfahren dar. Der neue Funktionswert wird wie folgt berechnet:

\begin{equation*}
   x_{n+1} = x_n + \theta \Delta t \cdot f(t_{n+1}, x_{n+1}) + (1-\theta) \Delta t \cdot f(t_{n}, x_{n}) \; .
\end{equation*}

Der prinzipielle Ablauf der Zeitintegration entspricht dem des impliziten Euler-Verfahrens. Für spezielle Werte von $\theta$ ergeben sich bekannte Verfahren:

| $\theta$ | Verfahren | Fehlerordnung |
| -------- | ---------- | -------------|
| 0 | explizites Euler-Verfahren | 1 |
| 0.5 | Crank-Nicolson-Verfahren | 2 |
| 1 | implizites Euler-Verfahren | 1 |

### Implizite Zeitintegration in der Strukturdynamik

Die populärste Methode der impliziten Zeitintegration ist das **Newmark-Verfahren** {cite}`newmark1959method`. Es ist eine Methode zweiter Ordnung, die die Beschleunigung als unbekannte Größe behandelt. Die Methode ist stabil und wird oft in der Praxis verwendet. Das Verfahren basiert auf einer Taylorreihenentwicklung der Verschiebung und der Geschwindigkeit:

\begin{align}
\bm{u}_{n+1} & = \bm{u}_n + \Delta t \dot{\bm{u}}_n + \frac{(\Delta t)^2}{2} \ddot{\bm{u}} + \mathcal{O}(\Delta t^3) \\
\dot{\bm{u}}_{n+1} & = \dot{\bm{u}}_n + \Delta t \ddot{\bm{u}} + \mathcal{O}(\Delta t^2)
\end{align}

Die Terme höherer Ordnung werden hierbei zu 0 gesetzt und für die noch unbekannten Beschleunigungen $\ddot{\bm{u}}$ werden lineare Ansätze getroffen:

\begin{align}
\bm{u}_{n+1} & = \bm{u}_n + \Delta t \dot{\bm{u}}_n + \frac{(\Delta t)^2}{2} \left[ (1-2\beta) \ddot{\bm{u}}_n + 2\beta \ddot{\bm{u}}_{n+1} \right] \\
\dot{\bm{u}}_{n+1} & = \dot{\bm{u}}_n + \Delta t \left[(1-\gamma)\ddot{\bm{u}}_{n} + \gamma \ddot{\bm{u}}_{n+1} \right]
\end{align}

In diesen Vorschriften tauchen auch Terme der Beschleunigung zum Zeitpunkt $n+1$ auf - diese sind noch unbekannt, weshalb es sich um ein implizites Integrationsverfahren handelt. Die Parameter $\beta$ und $\gamma$ sind frei wählbar und bestimmen die Genauigkeit und das Verhalten des Verfahrens. In {cite}`zienkiewicz2005finite` werden folgende Werte empfohlen:

\begin{align}
0 \leq &\beta \leq 0.5\\
0 \leq & \gamma \leq 1
\end{align}

Für die Lösung der Bewegungsgleichung wird die Approximation der Verschiebung und der Geschwindigkeit in Abhängigkeit der Verschiebung allein umgestellt.

\begin{align}
 \ddot{\bm{u}}_{n+1} & = \frac{1}{\beta \Delta t^2} \left( \bm{u}_{n+1} - \bm{u}_n \right) - \frac{1}{\beta \Delta t} \dot{\bm{u}}_n -  \frac{1-2\beta} {2\beta} \ddot{\bm{u}}_n \\
\dot{\bm{u}}_{n+1} & = \frac{\gamma}{\beta \Delta t} \left( \bm{u}_{n+1} - \bm{u}_n\right) + \left(1-\frac{\gamma}{\beta}\right) \dot{\bm{u}}_n + \Delta t \left(1-\frac{\gamma}{2\beta}\right) \ddot{\bm{u}}_n
\end{align}

Eingesetzt in die Bewegungsgleichung:

\begin{equation*}
\bm{M} \ddot{\bm{u}}_{n+1} + \bm{C} \dot{\bm{u}}_{n+1} + \bm{K} \bm{u}_{n+1} = \bm{F}_{n+1}
\end{equation*}

liefert:

\begin{align}
\underbrace{\left[\bm{M} \frac{1}{\beta \Delta t^2} + \bm{C} \frac{\gamma}{\beta \Delta t}+ \bm{K}\right]}_{K\rs{eff}} \bm{u}_{n+1} &= \bm{F}_{n+1} - \bm{M} \bm{b}_1 - \bm{C} \bm{b}_2 \\
\bm{b}_1 & = -\frac{1}{\beta \Delta t^2} \bm{u}_n- \frac{1}{\beta \Delta t} \dot{\bm{u}}_n - \frac{1-2\beta}{2 \beta} \ddot{\bm{u}}_n \\
\bm{b}_2 &= -\frac{\gamma}{\beta \Delta t} \bm{u}_n - \left( 1- \frac{\gamma}{\beta} \right)\dot{\bm{u}}_n - \left(1-\frac{\gamma}{2\beta}\right) \Delta t \ddot{\bm{u}}_n
\end{align}

Für jeden Zeitschritt wird dann das Gleichungssystem:

\begin{equation}
\bm{u}_{n+1} = \bm{K}\rs{eff}^{-1} \left[ \bm{F}_{n+1} - \bm{M} \bm{b}_1 - \bm{C} \bm{b}_2 \right]
\end{equation}

gelöst.

```{figure} images/Federschwinger_NM.png
---
name: Loesung_FederMasseSchwinger_numerisch_NM
alt: Analytische und numerische Lösung des Feder Masse Systems im Vergleich Newmark Verfahren
width: 500px
---
Analytische und numerische Lösung des Feder-Masse-Schwingers im Vergleich - Newmark Verfahren.
```

### Zusammenfassung Explizite vs. Implizite Zeitintegration

 Bei der Analyse dynamischer Probleme muss die Zeit diskretisiert werden, was durch Zeitintegrationsmethoden erreicht wird. Es gibt zwei Hauptklassen von Zeitintegrationsverfahren: explizite und implizite Methoden. Die explizite Zeitintegration berechnet den Systemzustand direkt zum nächsten Zeitschritt basierend auf aktuellen und vergangenen Informationen.
Sie erfordert keine Lösung eines linearen Gleichungssystems. Das macht sie einfach zu implementieren. Aufgrund ihrer bedingten Stabilität ist ein sehr kleiner Zeitschritt erforderlich, besonders bei steifen Systemen. Die implizite Zeitintegration berechnet den nächsten Systemzustand unter Einbeziehung des neuen Zustands selbst, was eine iterative Lösung eines linearen oder nichtlinearen Gleichungssystems bedeutet. Implizite Methoden bieten unbedingte Stabilität, was größere Zeitschritte ermöglicht, auch bei steifen Systemen.

```{figure} images/Ansys_implicit_Explicit.png
---
name: Anwendung_Explizit_Implizit
alt: Anwendung der expliziten und impliziten Zeitintegration
width: 700px
---
Anwendung der expliziten und impliziten Zeitintegration [Ansys Education Resources](https://www.ansys.com/de-de/academic/educators/education-resources/introduction-to-explicit-dynamics-using-ls-dyna)
```

### Dämpfung in der Strukturmechanik

In realen Strukturen wird Schwingungsenergie durch verschiedene physikalische Mechanismen dissipiert – etwa durch Materialreibung, Fugendämpfung an Verbindungen, Abstrahlung in angrenzende Medien oder viskose Effekte. Eine exakte Modellierung all dieser Mechanismen ist in der Praxis meist nicht möglich. Daher werden in der FEM vereinfachte Dämpfungsmodelle verwendet, die das globale Dissipationsverhalten der Struktur hinreichend genau abbilden. Die nachfolgenden Ausführungen basieren vornehmlich auf dem Lehrbuch {cite}`bathe2006finite`.

Die allgemeine Bewegungsgleichung lautet:

\begin{equation*}
\bm{M} \ddot{\bm{u}} + \bm{C} \dot{\bm{u}} + \bm{K} \bm{u} = \bm{f}(t)
\end{equation*}

Die zentrale Herausforderung besteht darin, die Dämpfungsmatrix $\bm{C}$ geeignet zu bestimmen.

#### Rayleigh-Dämpfung (proportionale Dämpfung)

Das in der Praxis am häufigsten eingesetzte Modell ist die **Rayleigh-Dämpfung** (auch proportionale Dämpfung genannt). Dabei wird die Dämpfungsmatrix als Linearkombination von Massen- und Steifigkeitsmatrix angesetzt:

```{math}
:label: eq_rayleigh
\begin{equation}
\bm{C} = \alpha \, \bm{M} + \beta \, \bm{K}
\end{equation}
```

Die Parameter $\alpha$ und $\beta$ werden als **Rayleigh-Koeffizienten** bezeichnet. Dieser Ansatz hat den großen Vorteil, dass die Dämpfungsmatrix mit den Eigenvektoren des ungedämpften Systems diagonalisierbar ist (modale Entkopplung bleibt erhalten).

##### Zusammenhang mit dem modalen Dämpfungsgrad

Für die $i$-te Eigenform mit Eigenkreisfrequenz $\omega_i$ ergibt sich der modale Dämpfungsgrad $\xi_i$ zu:

```{math}
:label: eq_rayleigh_xi
\begin{equation}
\xi_i = \frac{\alpha}{2 \omega_i} + \frac{\beta \, \omega_i}{2}
\end{equation}
```

Daraus lassen sich wichtige Eigenschaften ablesen:

- Der **Massenanteil** $\alpha$ dämpft vorwiegend die **niedrigen Frequenzen** (niederfrequente Moden).
- Der **Steifigkeitsanteil** $\beta$ dämpft vorwiegend die **hohen Frequenzen** (hochfrequente Moden).

##### Bestimmung der Rayleigh-Koeffizienten

In der Regel werden $\alpha$ und $\beta$ so bestimmt, dass für zwei ausgewählte Eigenfrequenzen $\omega_1$ und $\omega_2$ die gewünschten Dämpfungsgrade $\xi_1$ und $\xi_2$ erreicht werden. Aus Gleichung {eq}`eq_rayleigh_xi` folgt das Gleichungssystem:

```{math}
:label: eq_rayleigh_bestimmung
\begin{equation}
\begin{pmatrix} \frac{1}{2\omega_1} & \frac{\omega_1}{2} \\ \frac{1}{2\omega_2} & \frac{\omega_2}{2} \end{pmatrix}
\begin{pmatrix} \alpha \\ \beta \end{pmatrix}
=
\begin{pmatrix} \xi_1 \\ \xi_2 \end{pmatrix}
\end{equation}
```

Häufig wird vereinfachend $\xi_1 = \xi_2 = \xi$ angenommen (gleicher Dämpfungsgrad für beide Referenzfrequenzen). Dann ergibt sich:

\begin{align}
\alpha &= \frac{2 \xi \, \omega_1 \omega_2}{\omega_1 + \omega_2} \\
\beta &= \frac{2 \xi}{\omega_1 + \omega_2}
\end{align}

```{important}
**Praktischer Hinweis:** Die beiden Referenzfrequenzen $\omega_1$ und $\omega_2$ sollten den relevanten Frequenzbereich der Anregung einschließen. Zwischen diesen beiden Frequenzen ist die Dämpfung annähernd konstant. Außerhalb dieses Bereiches weicht der tatsächliche Dämpfungsgrad deutlich vom gewünschten Wert ab – bei sehr niedrigen Frequenzen dominiert der Massenanteil, bei sehr hohen Frequenzen der Steifigkeitsanteil.
```

##### Sonderfälle der Rayleigh-Dämpfung

| Bezeichnung | Parameter | Wirkung |
| ----------- | --------- | ------- |
| Reine Massendämpfung | $\alpha > 0, \; \beta = 0$ | Starke Dämpfung bei niedrigen Frequenzen, hohe Frequenzen nahezu ungedämpft |
| Reine Steifigkeitsdämpfung | $\alpha = 0, \; \beta > 0$ | Starke Dämpfung bei hohen Frequenzen, niedrige Frequenzen nahezu ungedämpft |
| Volle Rayleigh-Dämpfung | $\alpha > 0, \; \beta > 0$ | Kontrollierte Dämpfung über einen definierten Frequenzbereich |


#### Modale Dämpfung

Bei der modalen Analyse (Modenüberlagerung) wird die Dämpfung häufig nicht über eine explizite Dämpfungsmatrix $\bm{C}$ beschrieben, sondern direkt über **modale Dämpfungsgrade** $\xi_i$ für jede Eigenform $i$. Dieses Vorgehen ist besonders praktisch, weil:

- experimentell bestimmte Dämpfungswerte (z.B. aus Modalanalyse-Messungen) direkt verwendet werden können,
- für jede Mode ein individueller Dämpfungsgrad vorgegeben werden kann,
- keine vollbesetzte Dämpfungsmatrix aufgebaut werden muss.

Die entkoppelte modale Bewegungsgleichung für die $i$-te Mode lautet:

\begin{equation*}
\ddot{y}_i + 2 \xi_i \omega_i \dot{y}_i + \omega_i^2 y_i = \phi_i^T \bm{f}(t)
\end{equation*}

wobei $y_i$ die modale Koordinate und $\phi_i$ der $i$-te Eigenvektor ist.

```{tip}
**Typische Dämpfungsgrade** in der Praxis:

| Material / Struktur | Dämpfungsgrad $\xi$ |
| ------------------- | ------------------- |
| Stahl (geschweißt) | 0.01 – 0.02 |
| Stahl (geschraubt) | 0.02 – 0.05 |
| Aluminium | 0.005 – 0.01 |
| Beton | 0.02 – 0.05 |
| Gummi / Elastomere | 0.05 – 0.15 |
| Verbundwerkstoffe (CFK) | 0.01 – 0.03 |
```


#### Strukturelle Dämpfung (hysteretische Dämpfung)

Ein weiteres in der Praxis relevantes Modell ist die **strukturelle** oder **hysteretische Dämpfung**. Sie wird vor allem in der Frequenzbereichsanalyse (harmonische Analyse) verwendet. Die Dämpfungskraft ist hierbei proportional zur Verschiebung (nicht zur Geschwindigkeit):

\begin{equation*}
\bm{M} \ddot{\bm{u}} + (1 + i \eta) \bm{K} \bm{u} = \bm{f}(\omega)
\end{equation*}

wobei $\eta$ der **Verlustfaktor** (loss factor) und $i = \sqrt{-1}$ die imaginäre Einheit ist. Dieses Modell eignet sich gut für Werkstoffe, deren Energiedissipation pro Schwingungszyklus frequenzunabhängig ist (z.B. Metalle bei kleinen Amplituden).

```{important}
**Achtung:** Die strukturelle Dämpfung ist **nur im Frequenzbereich** physikalisch sinnvoll definiert. Im Zeitbereich ist sie nicht kausal und führt zu nicht-physikalischem Verhalten. Für transiente Analysen muss auf viskose Modelle (Rayleigh, modale Dämpfung) zurückgegriffen werden.
```


#### Empfehlungen für den Berechnungsingenieur

Die Wahl des Dämpfungsmodells hängt vom Analysetyp und den verfügbaren Daten ab:

| Analysetyp | Empfohlenes Modell | Bemerkung |
| ---------- | ------------------ | --------- |
| Transiente Analyse (direkte Integration) | Rayleigh-Dämpfung | Einfach zu implementieren, Referenzfrequenzen sorgfältig wählen |
| Modenüberlagerung (transient oder harmonisch) | Modale Dämpfung | Experimentelle Daten können direkt genutzt werden |
| Harmonische Analyse (Frequenzbereich) | Strukturelle Dämpfung oder modale Dämpfung | Verlustfaktor $\eta \approx 2\xi$ für kleine Dämpfung |
| Crash / Hochdynamik (explizit) | Massen-Dämpfung oder keine Dämpfung | Numerische Dissipation oft ausreichend |

```{warning}
**Häufige Fehlerquellen in der Praxis:**

1. **Zu hohe Rayleigh-Dämpfung bei hohen Frequenzen:** Wenn $\beta$ zu groß gewählt wird, werden hochfrequente Moden überdämpft. Dies kann physikalisch relevante Antworten unterdrücken.
2. **Referenzfrequenzen außerhalb des Anregungsbereichs:** Die Rayleigh-Koeffizienten sollten immer auf den relevanten Frequenzbereich der Anregung abgestimmt sein.
3. **Verwechslung von $\xi$ und $\eta$:** Für kleine Dämpfung gilt näherungsweise $\eta \approx 2\xi$. Diese Umrechnung wird in der Praxis häufig benötigt, wenn Herstellerdaten als Verlustfaktor angegeben sind.
4. **Vernachlässigung der Dämpfung:** Insbesondere bei Resonanzproblemen führt das Weglassen der Dämpfung zu unrealistisch hohen Amplituden.
```


### Beispiel: Sprungbrett mit impliziter Zeitintegration

In diesem Beispiel wird die Bewegung eines Sprungbretts nach einem Sprung simuliert. Die Kraft von 500 N wird innerhalb von 0,1 s aufgebracht und danach innerhalb 0,1 s wieder abgebaut. Die Simulation wird mit einer impliziten Zeitintegration vom **Newmark**-Typ durchgeführt. 

```{figure} images/Sprungbrett_Kraft.png
---
name: Sprungbrett_Kraft
alt: Sprungbrett mit Lastaufbringung
width: 700px
---
Sprungbrett mit Lastaufbringung
```
Eine effiziente Methode der linearen Dynamik ist die Modenüberlagerung - siehe hierzu: [Modenüberlagerung in transienter Berechnung](./Modalanalyse.md#modenüberlagerung-in-transienter-berechnung)


```{figure} images/NM_Sprungbrett.png
---
name: Sprungbrett_Loesung
alt: Vertikale Verschiebung
width: 700px
---
Vertikale Verschiebung des Sprungbretts in Abhängigkeit der Zeit.
```
