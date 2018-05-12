# Surface Detail

We will look at two methods for adding **surface detail** to rendered surfaces: texture mapping and bump mapping. A **texture** is defined as a 2D array of *texels*, often read from an image file or hardcoded for regular patterns. A texture is defined in its own coordinate system, conventionally referred to as {% math %}(u, v){% endmath %} or {% math %}(s, t){% endmath %}.

## Mapping texture per-polygon
We associate {% math %}(u, v){% endmath %} texture coordinates with each {% math %}(x, y, z){% endmath %} vertex of a polygon and interpolate the texture coordinates during scan-conversion, then we **blend** the pixel colour with the texture colour. We perform texture mapping as part of rasterisation, sample the texture at each pixel.

#### Meshes and seams
In order to realistically texture a mesh, we often have to use multiple textures in different places, which can give rise to ugly *seams*. One solution is to use textures that are *seamless*, so the edge of one exactly matches the edge of another.

## Resolution mismatches
Textures can be used to add accurate illumination to a real-life scene offline, computing accurate diffuse illumination model of the scene using a global model (radiosity) and save rendered surfaces and textures, called **lightmaps**, or in real-time, applying lightmap textures. A **resolution mismatch** occurs when the texture resolution does not match the pixel resolution.

#### pixel resolution > texture resolution
As the pixel resolution is greater than the texture resolution, neighbourhoods of pixels will happen to map to the same texel. Two of the approaches include no filter and bilinear interpolation filter.

##### No filter
We simply select the texels which the pixel maps to, resulting with a blocky looking image.

##### Bilinear interpolation filter
We compute a texel colour from adjacent texels, **averaging** horizontally and vertically, resulting in a smoother looking image.

#### pixel resolution < texture resolution
In this case, **adjacent** pixels may map to texels **far apart** in the texture, leading to *missing detail* (aliasing). In animated sequences especially, we see unpleasant aliasing effects, as pixels *pop* on and off, or change colour unexpectedly in each frame.

##### Mipmap filtering
One technique to minimise this effect is *mipmapping*. The idea is simple, the further away from the viewpoint, the less detail we need. So, we use a **set** of texture maps, and **select** which map to use, according to the **distance** of the pixel from the viewer. Starting from the original texture, we repeatedly create smaller versions of it, downsampling by {% math %}1/2{% endmath %} each time, until we reach a {% math %}1 \times 1{% endmath %} texture.

When rendering, we select one of the textures according to the **distance** of the pixel from the viewpoint. Alternatively, we can also choose the **two closest** textures and do bilinear interpolation for extra smoothness. The result of mipmapping is that we see blurring in distant polygons rather than aliasing.