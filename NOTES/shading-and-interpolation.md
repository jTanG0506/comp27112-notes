# Shading and Interpolation

We will look at three shading methods: **flat** (constant), **Gouraud** (intensity) and **Phong** (normal-vector).

## Flat shading
Flat shading is the simplest approach as we simply compute colour {% math %}C{% endmath %} at one vertex and use it for all pixels in the polygon. Each polygon is unformly coloured according to its orientation and we can clearly see the mesh.

#### Mach Banding
The **mach banding** effect consists in the edges between strips of different intensities to stand out. It exaggerates the contrast between edges of the slightly differing shades of gray, as soon as they contact one another, by triggering edge-detection in the human visual system.