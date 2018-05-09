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