---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive torus pattern

**Set up:** Chain $\left\lfloor\frac{2\pi(R-r)}{w}\right\rfloor$ stitches and join into a round.

**Row n:** Crochet $\left\lfloor \frac{2\pi}{w}\left(R-r\cos\left(\frac{n\pi}{N}\right)\right)\right\rfloor$ stitches around. Taking care to spread increases as evenly as possible.

### Example

:::{code-cell} python
:label: enter-R
:tags: [remove-input]
from ipywidgets import interact, interactive, fixed, interact_manual
import ipywidgets as widgets
import numpy as np

# Major & minor radii
R = 4

def F(R: float):
    return R

interact(F, R=4)
:::