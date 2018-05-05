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