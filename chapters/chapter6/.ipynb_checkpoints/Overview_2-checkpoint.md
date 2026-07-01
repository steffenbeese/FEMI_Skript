---

jupytext:  
text\_representation:  
extension: .md  
format\_name: myst  
format\_version: 0.13  
jupytext\_version: 1.16.7  
kernelspec:  
display\_name: Python 3 (ipykernel)  
language: python  
name: python3

---

+++ {"editable": true, "slideshow": {"slide\_type": "skip"}}

# Zeitabhängige Problemstellungen in der FEM

Bisher haben wir mit der FEM statische oder quasistatische Problemstellungen gelöst. Wenn die zeitliche Änderung der Belastung jedoch schnell erfolgt und Trägheitseffekte nicht mehr vernachlässigt werden können, dann müssen wir dies auch in der Simulation abbilden können. Wir sprechen dann von einer transienten FEM-Analyse. Zur Abgrenzung sei erwähnt, dass nicht jedes zeitabhängige Problem eine transiente Analyse erfordert. Sind zum Beispiel die Lasten