---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Crochet maths

A place for Rosie to write about maths and crochet. 


:::{figure} figs/crochet-increase-decrease.png
:width: 400
:label: fig:crochet-inc

Crochet increases and decreases.
:::


:::{code-cell} 
import ipywidgets as widgets
R = widgets.IntSlider(
    value=7,
    min=0,
    max=10,
    step=1,
    description='R:',
    disabled=False,
    continuous_update=False,
    orientation='horizontal',
    readout=True,
    readout_format='d'
)
:::

Major radius R = {eval}``slider.value``. You can change with this slider: {eval}``slider``