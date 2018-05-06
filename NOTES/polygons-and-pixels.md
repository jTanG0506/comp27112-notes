# Polygons and pixels

In 3D graphics, the most common building block is the **polygon** and the most common polygon is the **triangle**. A polygon is the space bounded by a set of edges between pairs of verticies in a ordered set. **Structured** polygons maintain a hierarchy of structure: Object {% math %}\rightarrow{% endmath %} Surfaces {% math %}\rightarrow{% endmath %} Polygons {% math %}\rightarrow{% endmath %} Edges / Vertices

OpenGL only renders **convex** polygons, those whose interior angles are each less than {% math %}180\degree{% endmath %}. A non-convex polygon is known as a **concave** polygon. For concave polygons, GLU provides functions to tessellate them into a set of convex polygons. We say that a polygon is **planar** if all its vertices lie in a single plane - for example, all triangles are planar.

#### Surface normal
The **surface normal** is a vector perpendicular to the plane of the polygon, we can use this to give a polygon a distinguishable *front* and *back* and also to describe its **orientation** in 3D space.

Orientation is an essential property used extensively in lighting calculations, collisions, culling, etc.

##### Finding the surface normal
- Step 1: Choose a pair of sequential edges {% math %}E_1{% endmath %} and {% math %}E_2{% endmath %} and compute their vectors
- Step 2: Invert the direction of the first so they now emanate from their shared vertex
- Step 3: Their cross product gives the surface normal {% math %}N = E_2 \times -E_1{% endmath %}
- Step 4: Normalise {% math %}N{% endmath %}

#### Polygon soup
One way to represent a scene using polygons is to have a huge list of individual polygons, to colour and draw in order, requiring individual manipulation. This is known as a **polygon soup** and has a number of undesirable properties, such as:
- Waste of storage space as most models contain surfaces, not individual polygons, so we could share vertices rather than replicate them per polygon
- Loss of semantics: does a polygon belong to a cow? or a table?
- Leads to *brute force* rendering which makes interaction with the model complex

#### Meshes
**Meshes** are a linked group of polygons, which can be used to represent surfaces. They retain semantics, reduce storage by sharing vertices and edges, help with interaction and help with structuring the model so we can manipulate it more easily.

- **Triangle strips** are a collection of linked triangles, {% math %}N{% endmath %} linked triangles can be defined using {% math %}N + 2{% endmath %} vertices - compared with {% math %}3N{% endmath %} vertices if each triangle was defined separately
- **Triangle fans** are a collection of linked triangle that share a same vertex, which again be defined using {% math %}N + 2{% endmath %} vertices instead of {% math %}3N{% endmath %}
- **Quadrilateral strips** are a collection of links quadrilaterals, {% math %}N{% endmath %} quadrilaterals can be defined using {% math %}2N + 2{% endmath %} vertices, compared with {% math %}4N{% endmath %} separate vertices
- **Quadrilateral meshes** are a collection of linked quadrilaterals, which is commonly used in terrain modelling and for approximating curved surfaces. {% math %}N \times M{% endmath %} quadrilaterals can be defined using {% math %}(N + 1) \times (M + 1){% endmath %} vertices, compared with {% math %}4MN{% endmath %} separate vertices

:heavy_exclamation_mark: Quadrilaterals must be **tessellated** into triangles during rendering

#### Scan conversion
**Scan conversion** is the process of converting polygon edges in world coordinates into lines to display on a screen.

##### Scan-converting a line
To scan-convert a line, we sample the true geometry of the line, and **approximate** it using the nearest pixels available. Bresenham (1965) developed a fast algorithm using only integer arithmetic, which is still in use today.

##### Scan-converting a polygon
When scan-converting a polygon, we know the polygon has been transformed by the viewing pipeline, so we know its {% math %}(x, y, z){% endmath %} vertex coordinates in screen space, where the {% math %}(x, y){% endmath %} coordinates correspond to a pixel position and the {% math %}z{% endmath %} coordinate is a measure of the vertex's distance from the eye.

##### Naïve approach
One approach would be to scan-convert each of the edges in turn and process each row of pixels and fill in the remaining interior pixels.

##### Sweep-line algorithm
This algorithm steps down a pair of edges, starting in this example at {% math %}(x_1, y_1){% endmath %}, then goes down scanline by scanline, finding the start and end of the part of the scanline inside the triangle. This algorithm is efficient because we only need to coompute the gradients of the edges {% math %}px{% endmath %} and {% math %}qx{% endmath %} once, at the beginning. However, it is a floating point algorithm, so we do need keep rounding to the pixel grid.

#### Hidden surface removal
When modelling a 3D world from a particular viewpoint, there will be some parts of the world we can see, and some parts we cannot see because they may be hehind other surfaces. There are two main approaches to solving this problem:
- We can solve it in **world space**, i.e. we can try to work it out geometrically in 3D before drawing the result, this was the earlier approaches that was used and it is extremely hard
- We can do it in **display space**, so, during scan-conversion, whenever we generate a pixel **P**, we determine whether some **other** 3D object, nearer to the eye, **also** maps to **P**. This is now the standard approach.

#### The z-buffer (depth-buffer)
The **z-buffer** keeps a record of the z-value of each pixel.

```
Initialise each pixel to the desired background colour
Initialise each z-buffer entry to MAXDEPTH
for each pixel P generated during scan-conversion of an object do
  if z-coordinate of P < ZBUFFER[P] then
    compute colour of P
    store colour in P
    store z-coordinate of P in ZBUFFER[P]
  else
    do nothing (since something else has already been mapped to P and is closer)
```

:heavy_exclamation_mark: A lack of precision in the z-buffer leads to incorrect rendering of pixels with similar or identical z-values - this is known as **z-fighting** or **stitching**