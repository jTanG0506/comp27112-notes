# Projections

The process of converting all 3D coordinates into suitable 2D coordinates for displaying on the screen is known as **projection**.

## Parallel projection
The **projection** is the set of points at which the **projectors intersect** the projection plane. Parallel edges remain parallel in the projection, however angles between edges may be **distorted**. The centre of projection is at **infinity**, hence the projectors are parallel.

#### Orthographic projection
In orthographic parallel projection, projects are **perpendicular** to the projection plane. The projection plane is **parallel** to a plane {% math %}(XY, XZ, YZ){% endmath %} of the world. The projected image shows only two of the three axis, and no distortion of lengths or angles, which makes it useful for software such as CAD.

##### Example
Suppose we have a projection plane located at {% math %}z = 0{% endmath %} and the plane is parallel to the XY plane. A point {% math %}(x, y, z){% endmath %} will project to {% math %}(x_p, y_p, z_p){% endmath %} on the projection plane. By definition {% math %}(x_p, y_p, z_p) = (x, y, 0){% endmath %}.

Notice that this process is the same as scaling by {% math %}(1, 1, 0){% endmath %} and the projection matrix will be singular, since we have lost information about the *z*-coordinate of such point.

#### Axonometric projection
In axonometric parallel projection, projectors are **perpendicular** to the projection plane. The projection plane can have **any orientation** and all three axes can be seen.

- It is **isometric** if the projection plane is symmetrical to 3 of the {% math %}(x, y, z){% endmath %} axes
- It is **dimetric** if the projection plane is symmetrical to 2 of the {% math %}(x, y, z){% endmath %} axes
- It is **trimetric** if the projection plane is place arbitrarily (symmetrical to 1 or 0 of the {% math %}(x, y, z){% endmath %} axes)

#### Oblique projection
In **oblique** parallel projection, projectors can make **any angle** with the projection plane and the projection plane can have **any orientation** relative to the object being viewed. We see all three axes but there will be distortions of lengths or angles.

## Perspective projection
The **projection** is a set of points at which the projectors intersect the projection plane. As the centre of projection is **a point**, the projectors will **converge**. Objects further away from the centre of perspective become smaller, edges that were parallel may converge and angles between edges may be **distorted**.

The number of {% math %}(x, y, z){% endmath %} axes parallel to the projection plane determines the number of **vanishing points** in the projected image.
- **1-point perspective** with 2 parallel axes
- **2-point perspective** with 1 parallel axis
- **3-point perspective** with 0 parallel axis

#### Computing perspective
Say we have a projection plane located at {% math %}z = d{% endmath %}, and parallel to the {% math %}XY{% endmath %} plane, we want to find the point that {% math %}(x, y, z){% endmath %} will project to, on the projection plane, {% math %}(x_p, y_p, z_p){% endmath %}. By definition, {% math %}z_p = d{% endmath %}. By looking at the top view, looking down the y-axis, by similar triangles, we have {% math %}\frac{x_p}{d} = \frac{x}{z}{% endmath %} and so {% math %} x_p = \frac{dx}{z}{% endmath %} and similarly, on a side view, with our eye at the origin and looking along the x-axis, we have {% math %}y_p = \frac{dy}{z}{% endmath %}.

We can express this transformation as a {% math %}4 \times 4{% endmath %} matrix.
{% math %}
\begin{bmatrix}
    x \\
    y \\
    z \\
    z/d
\end{bmatrix}
=
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    w'
\end{bmatrix}
=
\begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 1/d & 0
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}

Recall that we must always end up with a 3D point which has a {% math %}w = 1{% endmath %}, so we must divide all elements by {% math %}w{% endmath %} and this is known as **perspective division**.

#### 3-point projection
We can also derive a matrix which expresses the most general case, **3-point projection**, where the projection plane is not parallel to any of the {% math %}XY, XZ{% endmath %} or {% math %}YZ{% endmath %} planes. The projection plane intersects the {% math %}X, Y{% endmath %} and {% math %}Z{% endmath %} axes at {% math %}(d_x, d_y, d_z){% endmath %}.
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    w'
\end{bmatrix}
=
\begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    1/d_x & 1/d_y & 1/d_z & 0
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}