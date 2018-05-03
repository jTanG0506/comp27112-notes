# Introduction to Image Synthesis

A **vector display** is a type of display in which an electron beam draws lines and text as instructed. As the name suggests, it is only capable of drawing vectors and is usually monochrome.

A **raster graphic** uses a 2D array of pixels to represent the image. Images must be sampled and the more samples, the better fidelity. However, raster graphics will always be an approximation.

### What is OpenGL?
OpenGL stands for **Open Graphics Library** is a cross-platform and language-independent API for doing 2D and 3D computer graphics. It is the most widely used 2D and 3D graphics API, with common uses in industry, engineering, CAD, CGI, research, education, etc.

It's main features include 3D graphics (points, lines, polygons, ...), coordinate transformations, viewing camera, hidden surface removal, lighting and shading, texturing, pixel (image) operations and many more.

#### Double buffering
**Double buffering** is a technique for drawing graphics that show no (or less) stutter, tearing and other artifacts. As the name suggests, this is done by having two buffers, referred to as the **back buffer** and the **front buffer**. The idea is that the back buffer is only ever written to by the renderer and the front buffer is only ever read by the DAC (Digital to Analog Converter). The renderer writes new frames into the back buffer and then requests that the back and front buffers are swapped.

The swapping is done whilst the DAC is performing its vertical retrace, which is when it is has finished a complete sweep of the buffer and is resetting to the beginning again and this period is known as the **vertical retrace time**.

#### Fixed pipeline
| :white_check_mark: Pros | :negative_squared_cross_mark: Cons |
| ---- | ---- |
| it's simple to use and sufficient for many purposes | new algorithms and techniques can't be added |
| it is easy to get started in a short time for beginners | it's deprecated! |

#### Programmable pipeline
| :white_check_mark: Pros | :negative_squared_cross_mark: Cons |
| ---- | ---- |
| provides huge flexibility | for beginners, there is a significant start-up cost |
| it's the state-of-the-art, cutting edge |   |

### GLU and GLUT
OpenGL itself, only supports low-level rendering and for convenience, there are two supports libraries that *sit on top of* OpenGL: **GLU** and **GLUT**.

#### GLU
The **GL Utility Library** provides functions which *wrap up* lower level OpenGL graphics by providing helper functions to draw curves, surfaces, cyclinders, discs, quadrics, etc. It also provides utility functions for viewing, textures and tessellation.

#### GLUT
The **GL Utility Toolkit** provides functions to deal with interaction with the user, such as handling mouse and keyboard input and providing functions to create a simple menu system. GLUT also provides a number of functions for generating easily recognisable 3D geometric objects, such as spheres, tori, cones, cubes, tetrahedrons and even teapots!

:warning: **OpenGL**, **GLUT** and **GLU** are together loosely called **OpenGL**