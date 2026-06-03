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

The two patches are crocheted in UK double crochet (dc), with identical yarn, hook and tension, and with all stitches facing the same way (the crochet version of stockinette). For the overlayed graphs, every stitch gets a vertex, and two stitches are considered adjacent if they are next to one another on the same row, or if one stitch is worked into the top of the other. 

[](#fig:flat-crochet) contains no increases, and an example vertex is labelled $u$. [](#fig:inc-dec-crochet) has a 2dc increase on row 3 (vertex $s$) and a 1dc decrease on row 4 (vertex $t$). The overlayed graphs illustrate the effect of these increases and decreases on the degree of each vertex: $d(u)=4$, $d(s)=6$ and $d(t)=3$. 

The *voronoidal decomposition* of a triangle $T$ is defined as follows. Let $c(T)$ denote the centroid of $T$, that is, the intersection of the perpendicular bisectors of the edges. Depending on $T$, $c(T)$ may lie inside or outside of $T$. For each vertex $v$ of $T$, the associated voronoidal region is the quadrilateral with vertices $v$, $c(T)$ and the midpoints of the two edges incident at $v$:
:::{figure} figs/tri-voronoi.png
:label: fig:tri-voronoi
:width: 250

Triangle subdivided into voronoidal regions
:::

Note that if $T$ has an obtuse angle, then its voronoidal regions will not all lie strictly inside $T$.

::::{prf:definition} Mesh curvature
Let $M$ be a surface and $\Gamma$ a triangular mesh. Let $v$ be a vertex of $\Gamma$, and $T(v)$ the set of triangles incident at $v$. 

Consider the voronoidal regions associated with each triangle incident at $v$. Let $A(v)$ denote the combined area of these regions. Let $\theta_1,\ldots,\theta_n$ denote the angles at $v$.

:::{figure} figs/v-voronoi.png
:label: fig:v-voronoi
:width: 300

Example: vertex $v$ with combined voronoidal region $A(v)$ and incident angles $\theta_i$.
:::

The *mesh curvature* of $\Gamma$ at $v$ is 
:::{math}
:enumerated: true
:label: eq:mesh-curvature
K(v) := \frac{1}{A(v)}\left(2\pi - \sum_{i=1}^n\theta_i\right)
:::
::::

By the Gauss-Bonnet theorem, $K(v)$ provides a good approximation of the Gauss curvature of the underlying surface. For how good, see .... papers .... 

Let us return to the crochet of [](#fig:flat-crochet) and [](#fig:inc-dec-crochet). Naiively, tension is uniform and all stitches are the same size. This "should" mean that the voronoidal areas for each stitch are equal, and so are the angles incident at each stitch.  We can discuss the limitations of this assumption later. For now, we proceed.

In [](#fig:flat-crochet), the work is flat and the angles around every vertex sum to $2\pi$ radians. The voronoidal area is equal to $hw$, where $h$ and $w$ are the height and width of each stitch, respectively. In flat crochet, every stitch is adjacent to exactly $4$ stitches - that is, every interior vertex is order 4. At a site of increase or decrease, the order of the vertex goes up or or down according to the number of stitches gained or lost. For example, stitch $s$ in [](#fig:inc-dec-crochet) has order $6$, but (assuming perfect tension) each adjacent stitch is the same size and contributes the same voronoidal area. This means that the angles meeting at $s$ will instead sum to $3\pi$, and the associated voronoidal area will also be $1.5$ times bigger, $\frac{3hw}{2}$. The curvature at $s$ is therefore (approximately)
$$
K(s) = -\frac{2\pi}{3hw}.
$$


<!---:::::{grid} 1 1 3 3
::::{grid-item}
:::{figure} figs/flat-graph.png
:label: fig:flat-graph
:width: 140

Vertex $u$ and adjacent vertices (c.f. [](#fig:flat-crochet)).
:::
::::
::::{grid-item}
:::{figure} figs/inc-dec-graph1.png
:label: fig:inc-dec-graph1
:width: 150

Vertex $s$ and adjacent vertices (c.f. [](#fig:inc-dec-crochet)).
:::
::::
::::{grid-item}
:::{figure} figs/inc-dec-graph2.png
:label: fig:inc-dec-graph2
:width: 100

Vertex $t$ and adjacent vertices (c.f. [](#fig:inc-dec-crochet)).
:::
::::
:::::
--->


