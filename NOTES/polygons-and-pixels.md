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