# Introduction to Image Synthesis

A **vector display** is a type of display in which an electron beam draws lines and text as instructed. As the name suggests, it is only capable of drawing vectors and is usually monochrome.

A **raster graphic** uses a 2D array of pixels to represent the image. Images must be sampled and the more samples, the better fidelity. However, raster graphics will always be an approximation.

### What is OpenGL?
OpenGL stands for **Open Graphics Library** is a cross-platform and language-independent API for doing 2D and 3D computer graphics. It is the most widely used 2D and 3D graphics API, with common uses in industry, engineering, CAD, CGI, research, education, etc.

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