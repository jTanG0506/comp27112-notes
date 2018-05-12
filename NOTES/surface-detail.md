# Surface Detail

We will look at two methods for adding **surface detail** to rendered surfaces: texture mapping and bump mapping. A **texture** is defined as a 2D array of *texels*, often read from an image file or hardcoded for regular patterns. A texture is defined in its own coordinate system, conventionally referred to as {% math %}(u, v){% endmath %} or {% math %}(s, t){% endmath %}.

## Mapping texture per-polygon
We associate {% math %}(u, v){% endmath %} texture coordinates with each {% math %}(x, y, z){% endmath %} vertex of a polygon and interpolate the texture coordinates during scan-conversion, then we **blend** the pixel colour with the texture colour. We perform texture mapping as part of rasterisation, sample the texture at each pixel.

#### Meshes and seams
In order to realistically texture a mesh, we often have to use multiple textures in different places, which can give rise to ugly *seams*. One solution is to use textures that are *seamless*, so the edge of one exactly matches the edge of another.