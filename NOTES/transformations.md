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

#### Invertible matrices
- Two matrices {% math %}A{% endmath %} and {% math %}B{% endmath %} are said to be inverses of each other if {% math %}A \times B = I{% endmath %}, where {% math %}I{% endmath %} is the identity matrix
- For a matrix {% math %}M{% endmath %}, we write its inverse as {% math %}M^{-1}{% endmath %}, so {% math %}M \times M^{-1} = I{% endmath %}
- We say a matrix {% math %}M{% endmath %} is **singular** if it has no inverse, in other words, it cannot be undone. For example, if you project a 3D object onto the x-y plane, there is no way to revert this, as we have thrown away information

#### Model and World coordinate systems
- Often an object is defined in a **local modelling coordinate system**, for example, modelling a car wheel with an origin at the wheel centre
- **Modelling transformations** are used to **instance** multiple copies of an object in a scene, for example, translate and rotate the wheels onto a car body
- The entire car may then have further transformations applied, like translation to simulate its movement
- A global **world coordinate system** is used to specify the position of objects in the entire scene

#### Vector addition
To add two vectors of the same order, simply add them component wise, as follows. Vector addition can be used to move a point through space in a known direction.
{% math %}
\begin{bmatrix}
    x_1 \\
    y_1 \\
    z_1 \\
    1
\end{bmatrix}
+
\begin{bmatrix}
    x_2 \\
    y_2 \\
    z_2 \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    x_1 + x_2 \\
    y_1 + y_2 \\
    z_1 + z_2 \\
    1
\end{bmatrix}
{% endmath %}

#### Vector subtraction
To subtract two vectors of the same order, simply subtract them component wise, as follows. Vector subtraction results in the 'line' representation between two points.
{% math %}
\begin{bmatrix}
    x_1 \\
    y_1 \\
    z_1 \\
    1
\end{bmatrix}
-
\begin{bmatrix}
    x_2 \\
    y_2 \\
    z_2 \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    x_1 - x_2 \\
    y_1 - y_2 \\
    z_1 - z_2 \\
    1
\end{bmatrix}
{% endmath %}

#### Multiplication by a scalar
Multiply the individual components by a scalar {% math %}C{% endmath %}. Multiplying by a scalar moves a point along a vector by a given amount.
{% math %}
\begin{bmatrix}
    x_1 \\
    y_1 \\
    z_1 \\
    1
\end{bmatrix}
\times
C
=
\begin{bmatrix}
    x_1 \times C \\
    y_1 \times C \\
    z_1 \times C \\
    1
\end{bmatrix}
{% endmath %}

#### Vector normalization
**Normalization** is the process of taking a non-zero vector {% math %}V{% endmath %} and converting it into a vector {% math %}\hat{V}{% endmath %} **of length 1**, which points in the same direction. To do this, simply calculate the length {% math %}L{% endmath %} of {% math %}V{% endmath %} and divide its {% math %}x, y{% endmath %} and {% math %}z{% endmath %} components by this value. This is an essential operation in rendering.

#### Dot product (inner product)
The **dot product** is the scalar product of the individual components. For normalized vectors, this quantity is the cosine of the angle between them.

{% math %}
\begin{bmatrix}
    x_1 \\
    y_1 \\
    z_1 \\
    1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x_2 \\
    y_2 \\
    z_2 \\
    1
\end{bmatrix}
= (x_1 \times x_2) + (y_1 \times y_2) + (z_1 \times z_2)
{% endmath %}

#### Cross product (outer product)
The **cross product** is a vector, defined as follows:

{% math %}
\begin{bmatrix}
    x_1 \\
    y_1 \\
    z_1 \\
    1
\end{bmatrix}
\cdot
\begin{bmatrix}
    x_2 \\
    y_2 \\
    z_2 \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    y_1 \times z_2 - z_1 \times y_2 \\
    z_1 \times x_2 - x_1 \times z_2 \\
    x_1 \times y_2 - y_1 \times x_2 \\
    1
\end{bmatrix}
{% endmath %}