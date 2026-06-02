---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Crochet maths

A place for Rosie to write about maths and crochet. 

:::{toc} Contents
:context: project
:::


:::{figure} figs/crochet-increase-decrease.png
:width: 400
:label: fig:crochet-inc

Crochet increases and decreases.
:::

## Geometry of surfaces and amigurumi

Amigurumi style crochet is characterised by stacking rows of double crochet stitches directly on top of each other. Often worked in cotton, the resulting fabric is dense and close, holding its own shape. This makes it particularly well-suited to geometric analysis. Indeed, a piece of crochet is a polygonal mesh of a surface in 3 dimensional space, naturally parametrised by row and stitch counts. On the other hand, if we begin with a parametrised surface, a crochet pattern can be generated through a process of discretisation. 

In computer graphics, surfaces are typically rendered as meshes, for ease of storage and computation. Many geometric objects such as the Lapalace-Beltrami operator and Gauss curvature have discrete analogues developed for the purpose of analysing these meshes, and we can do the same for crochet.

### Increases, decreases and curvature
Anyone who has crocheted or knitted will appreciate that increases and decreases impact the curvature of the resulting fabric. The specific impact will depend on other factors such as stitch gauge and whether you work in rows or in the round. 

Consider the following crocheted surface patches.

:::::{grid} 1 1 2 2
::::{grid-item}
:::{figure} figs/flat-crochet.png
:label: fig:flat-crochet
:width: 300

Flat crochet
:::
::::
::::{grid-item}
:::{figure} figs/inc-dec-crochet.png
:label: fig:inc-dec-crochet
:width: 300

Crochet with increases and decreases
:::
::::
:::::

The two patches are crocheted in UK double crochet (dc), with identical yarn, hook and tension, and with all stitches facing the same way (the crochet version of stockinette). The graph overlays are constructed as follows: place one vertex on every stitch. Two vertices are connected by an edge if they are adjacent, which can happen in two ways
1. They are directly next to one another on a shared row,
2. One has been crocheted into the top of the other on the row previous.

[](#fig:flat-crochet) contains no increases, and an example vertex is labelled $\mathbf{u}$.

[](#fig:inc-dec-crochet) has a 2dc increase on row 3 (vertex $\mathbf{v}$) and a 1dc decrease on row 4 (vertex $\mathbf{w}$). The overlayed graphs illustrate the effect of these increases and decreases on the degree of each vertex: $d(\mathbf{u})=4$, $d(\mathbf{v})=6$ and $d(\mathbf{w})=3$. 

:::{prf:definition} Mesh curvature

:::