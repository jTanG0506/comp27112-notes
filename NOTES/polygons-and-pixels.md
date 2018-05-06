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