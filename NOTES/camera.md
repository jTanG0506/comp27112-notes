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

#### Clipping
Normally, we want to **clip** against the viewport to remove those parts of the primitives whose coordinates are outside the window and sometimes it's useful to use multiple windows and viewports, to help arrange items on the screen.

:warning: There is no actual camera - we imagined we have a camera and simulated it by **transforming the model**

#### Viewing in 3D
- In 2D graphics, we view our world by mapping from 2D world coordinates to 2D screen coordinates - which is easy and obvious
- However, in 3D graphics, in order to view our world, we have to somehow **reduce** our 3D information to 2D information, so it can be displayed on the 2D display - which is **not** so easy and obvious

#### The camera analogy
|   | Real World | Computer Graphics |
| - | ---------- | ----------------- |
| 1 | Arrange the scene into the desired composition | Set Modelling Transformation |
| 2 | Point the camera at the scene | Set Viewing Transformation |
| 3 | Choose the camera lens or adjust the zoom | Set Projection Transformation |
| 4 | Determine the size and shape of the final photograph | Set Viewport Transformation |

##### Duality of modelling and viewing
We can create the same view from a camera at a certain location and orientation by transforming the object - so it's impossible to tell if the camera was moved, or if the model was moved.

- For example, moving the model by {% math %}(x, y, z){% endmath %} is equivalent to moving the model by {% math %}(-x, -y, -z){% endmath %}
- When we *change the camera location and orientation*, we compute a viewing transformation which we then apply to the object

##### Specifying the camera
The *camera* is specified by:
- **Eye point** ({% math %}E{% endmath %}) - where the camera is located in 3D space
- **Centre of interest** ({% math %}C{% endmath %}) - a 3D point at which the *camera* is looking at
- **Up vector** ({% math %}V{% endmath %}) - direction of the camera, using a view up vector

##### Defining the camera coordinate system
We use {% math %}E, C, V{% endmath %} to derive a coordinate system for the *camera* and we call the axis of the camera's coordinate system {% math %}\hat{S} \, \hat{U} \, \hat{F}{% endmath %} where
- **F** is {% math %}C - E{% endmath %} - a normalised vector since it is only used to specify a direction
- **S** is \hat{(F \times U)}
- **S** is \hat{(F \times S)} (without any assumptions that V is orthogonal to F)

##### The viewing transformation
**The viewing transformation** {% math %}T_{c}^{-1}{% endmath %} moves and rotations the {% math %}\hat{S} \, \hat{U} \, \hat{F}{% endmath %} camera system to align with the origin of the world coordinates system, and thte idea is that if we apply {% math %}T_{c}^{-1}{% endmath %} to **objects**, we will get the **same view** as if we had a real camera at {% math %}E{% endmath %}.

To derive {% math %}T_{c}^{-1}{% endmath %}, the transformation that moves the camera coordinate system to the world origin, we do:
- Step 1: translate the origin of the camera system onto the origin of the world system
- Step 2: rotate the camera axis to be coincident with the world axes, with {% math %}\hat{F}{% endmath %} aligned with {% math %}-Z{% endmath %}
