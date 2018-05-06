# Camera

#### Viewing in 2D
The problem we have is how to map our 2D scene onto the display scene? We can specify a window in *world* coordinates, a *viewport* in screen coordinates and find the matrix {% math %}M_{view}{% endmath %} (**viewing transformation**) which transforms the window to the viewport.

##### Calculating the viewing transformation in 2D
We can find {% math %}M_{view}{% endmath %} very easily:
- {% math %}M_1{% endmath %}: Translate by {% math %}(-x_0, -y_0){% endmath %} to place the window at the origin
- {% math %}M_2{% endmath %}: Scale the window to be the same size as the viewport
- {% math %}M_3{% endmath %}: Shift to the viewport position

It follows that {% math %}M_{view}{% endmath %} = {% math %}M_3 \times M_2 \times M_1{% endmath %} and hence {% math %}P_{screen} = M_{view} \times P_{world}{% endmath %}

##### Example
:speak_no_evil: This is **simple** matrix multiplication and is not examinable.

{% math %}
\begin{bmatrix}
    1 & 0 & u_0 \\
    0 & 1 & v_0 \\
    0 & 0 & 1
\end{bmatrix}
\times
\begin{bmatrix}
    \frac{u_1 - u_0}{x_1 - x_0} & 0 & 0 \\
    0 & \frac{v_1 - v_0}{y_1 - y_0} & 0 \\
    0 & 0 & 1
\end{bmatrix}
\times
\begin{bmatrix}
    1 & 0 & -x_0 \\
    0 & 1 & -y_0 \\
    0 & 0 & 1
\end{bmatrix}
{% endmath %}

thus, we have that

{% math %}
P_{screen} = 
\begin{bmatrix}
    \frac{u_1 - u_0}{x_1 - x_0} & 0 & -x_0 * \frac{u_1 - u_0}{x_1 - x_0} + u_0 \\
    0 & \frac{v_1 - v_0}{y_1 - y_0} & -y_0 * \frac{v_1 - v_0}{y_1 - y_0} + v_0 \\
    0 & 0 & 1
\end{bmatrix}
* P_{world}
{% endmath %}
