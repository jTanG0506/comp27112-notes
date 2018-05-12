# Shading and Interpolation

We will look at three shading methods: **flat** (constant), **Gouraud** (intensity) and **Phong** (normal-vector).

## Flat shading
Flat shading is the simplest approach as we simply compute colour {% math %}C{% endmath %} at one vertex and use it for all pixels in the polygon. Each polygon is unformly coloured according to its orientation and we can clearly see the mesh.

#### Mach Banding
The **mach banding** effect consists in the edges between strips of different intensities to stand out. It exaggerates the contrast between edges of the slightly differing shades of gray, as soon as they contact one another, by triggering edge-detection in the human visual system.

## Gouraud shading
Gouraud shading uses intensity interpolation to smooth out the discontinuities between polygons. We can approximate the normal of the underlying *surface* by averaging the normals where polygons share vertices.

#### Gouraud's algorithm
First, we compute pixel colours, {% math %}C_a, C_b{% endmath %} and {% math %}C_c{% endmath %} of vertices {% math %}A, B{% endmath %} and {% math %}C{% endmath %}. For each scanlie, we average {% math %}C_a{% endmath %} and {% math %}C_c{% endmath %} in {% math %}C_{left}{% endmath %} and average {% math %}C_b{% endmath %} and {% math %}C_c{% endmath %} in {% math %}C_{right}{% endmath %} and then finally average {% math %}C_{left}{% endmath %} and {% math %}C_{right}{% endmath %} along the scanline.

The result of this is that the mesh now appears smooth, however the silhouette edges are still polygonal. Although this method is fast and efficient, specular highlights may be distorted or averaged away altogether, as Gouraud shading averages between **vertex** colours. Mach banding may still be visible. Sometimes edges are shaded away, thus edges must be tagged in the data structure to avoid interpolation across it.

## Phong's interpolation
Phong's interpolation is similar to Gouraud's algorithm, however, instead of interpolating colours, Phong suggested interpolating normal vectors. We interpolate the normal vector along the scanline and compute the illumination model for every pixel.

#### Phong's algorithm
Let {% math %}N_a, N_b{% endmath %} and {% math %}N_c{% endmath %} denote the normal vectors at vertices {% math %}A, B{% endmath %} and {% math %}C{% endmath %} respectively. For each scanline, we compute the average normal {% math %}N_{left}{% endmath %} from {% math %}N_a{% endmath %} and {% math %}N_c{% endmath %} and compute the average normal {% math %}N_{right}{% endmath %} from {% math %}N_b{% endmath %} and {% math %}N_c{% endmath %}, then we average {% math %}N_{left}{% endmath %} and {% math %}N_{right}{% endmath %} along the scanline and compute the colour of the pixel using the illumination model.

The result of Phong's algorithm is that specular highlights are rendered correctly, but rendering is more expensive.