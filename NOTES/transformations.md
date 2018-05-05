# Transformations

#### 3D Cartesian Coordinates
- A coordinate represents a point in space, measured with respect to an **origin** and a set of {% math %}X, Y, Z {% endmath %} **axes**.

#### Vectors
- A vector represents a **direction** in space, with respect to a set of {% math %}X, Y, Z {% endmath %} **axes**. It has a characteristic length and a vector of length 1 is known as a **unit vector**.

#### Coordinates and Vectors
- Both coordinates and vectors can be represented by a triple of {% math %}x, y, z {% endmath %} values.

- In the images above, the melon is **at** point **P** and the **spatial relationship** between the origin and the melon is described by the vector **V**.

- We can write a vector as either a column or a row: {% math %}
\begin{bmatrix}
    x \\
    y \\
    z
\end{bmatrix}
or
\begin{bmatrix}
    x & y & z
\end{bmatrix}
{% endmath %}

:warning: OpenGL uses column vectors, whereas other systems, like MATLAB, use row vectors. Altough the two representations are equivalent, a transformation matrix used with column vectors is the **transpose** of the equivalent matrix used with row vectors.

#### Geometric transformations
Geometry is defined as a set of vertices and we apply transformations to verticies to change them, for example, translation, scaling and rotation. To transform a whole shape, we transform all of its individual vertices.

### Transformations as matricies
In order to use a consistent matrix representation for all kinds of linear transformations, we need an extra coordinate {% math %}w{% endmath %}, thus we represent **transformations** as {% math %}4 \times 4{% endmath %} matrices. This {% math %}(x, y, z, w){% endmath %} form is called **homogeneous coordinates**. Usually this {% math %}w = 1{% endmath %} and if it isn't, then we just normalise it.

#### 3D Scaling
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    S_x & 0 & 0 & 0 \\
    0 & S_y & 0 & 0 \\
    0 & 0 & S_z & 0 \\ 
    0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}

#### 3D Translation
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    1 & 0 & 0 & T_x \\
    0 & 1 & 0 & T_y \\
    0 & 0 & 1 & T_z \\ 
    0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}

#### 3D Rotation (about x-axis)
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & \cos \theta & - \sin \theta & 0 \\
    0 & \sin \theta & \cos \theta & 0 \\ 
    0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}

#### 3D Rotation (about y-axis)
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    \cos \theta & 0 & \sin \theta & 0 \\
    0 & 1 & 0 & 0 \\
    - \sin \theta & 0 & \cos \theta & 0 \\ 
    0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}

#### 3D Rotation (about z-axis)
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    z' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    \cos \theta & - \sin \theta & 0 & 0 \\
    \sin \theta & \cos \theta & 0 & 0\\ 
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x \\
    y \\
    z \\
    1
\end{bmatrix}
{% endmath %}

#### Composite transformations
Say we wanted to scale an object by {% math %}(sx, sy, sz){% endmath %} about an arbitrary point {% math %}P (px, py, pz){% endmath %}. We split this into simpler steps:
- Step 1: Construct the translation {% math %}M_1{% endmath %} which shifts the object to the origin, by {% math %}(-px, -py, -pz){% endmath %}
- Step 2: Construct the matrix {% math %}M_2{% endmath %} which scales the object by {% math %}(sx, sy, sz){% endmath %} with respect to the origin
- Step 3: Construct the translation {% math %}M_3{% endmath %} which shifts the object back by {% math %}(px, py, pz){% endmath %}

The **composite transformation** is {% math %}M_3 \cdot M_2 \cdot M_1{% endmath %}. Notice that {% math %}M_1{% endmath %} is applied first, then {% math %}M_2{% endmath %}, then {% math %}M_3{% endmath %}.