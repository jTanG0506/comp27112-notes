# Local Illumination

Models of light-matter can interact in two ways, **locally** and **globally**. In a **local illumination model**, we treat each object in a scene separately from any other object, so reflections between objects are simply ignored. In a **global illumination model**, we treat all objects together and model the interactions between objects, well at least try to, as it is an extremely complex process. As digital images have finite precision, we can only **approximate** it to various degrees of fidelity.

## Reflectivity
There are three broad classes of reflection: diffuse reflection, perfect specular reflection and imperfect specular reflection.

##### Diffuse
**Diffuse** reflection is absorption and uniform re-radiation. Some wavelengths are absorbed, whilst others are reflected. Diffuse reflectors always absorb specific wavelengths and reflects others, which is why they have a fixed colour.

In a **perfect diffuse** reflection, incoming rays of lights are reflected across all angles on the surface and results in the surface looking dull, or matte. Examples of perfect diffuse reflectors include carpets, sponges, bricks and felt.

##### Specular
**Specular** reflection is reflection at the air/surface interface. To the first approximation, the colour of the specular reflection is the same colour as the light source.

A **perfect specular** reflection reflects an incoming ray like a perfect mirror, and so the surface must be smooth. An **imperfect specular** surface reflects an incoming ray across a small range of angles as the surface is irregular and will appear shiny, with highlights. Examples of **imperfect specular** reflectors include lacquer-coated aluminium (used on CDs), glazed ceramic and stainless steel.

:exclamation: Most surfaces exhibit a mixture of diffuse and specular reflection and we can model these effects with varying degrees of realism

## Illumination
We will look at three **sources** of illumination: ambient illumination, point illumination source at infinity (directional illumination) and point illumination source in the scene.

#### Ambient illumination
Consider an environment with a light source. Multiple reflections cause a general level of illumination in the scene, called **ambient illumination**. The amount of light diffusely reflected from a surface is {% math %}I_{\text{ambient}} = k_aI_a{% endmath %}, where {% math %}I_a{% endmath %} is the intensity of the ambient light and {% math %}0 \leq k_a \leq 1{% endmath %} is the **ambient reflection coefficient** of the surface. This term is a gross simplification of the true ambient illumination, which is **not** constant in the scene.

:exclamation: The effect of ambient illumination is that each object is uniformly illuminated, which means 3D information is lost

#### Directional lighting
Consider a light from a point source at infinity, then direction is our concern, and this is called **directional lighting**. We need to model the effects of different angles of incidence and different distances from the light source.

Consider light of intensity {% math %}I{% endmath %} and cross-sectional width {% math %}x{% endmath %} falling on a surface, so width {% math %}x{% endmath %} on surface receives all of intensity {% math %}I{% endmath %}. Now consider the light inclined at {% math %}\theta{% endmath %} then {% math %}x' = x / \cos \theta{% endmath %} and {% math %}x = x'\cos \theta{% endmath %}. So width {% math %}x{% endmath %} receives intensity {% math %}I\cos \theta{% endmath %}.

It follows that, the amount of diffusely reflected light is {% math %}I_{\text{diffuse}} = I_pk_d \cos \theta = I_pk_d(\hat{N} \cdot \hat{L}){% endmath %}, where {% math %}0 \leq k_d \leq 1{% endmath %} is the **diffuse reflection coefficient** which tells us how good a surface is at diffuse reflecting.

Physically, light intensity falls off with the square of the distance travelled. After travelling {% math %}d{% endmath %}, the original intensity {% math %}I_p{% endmath %} becomes {% math %}I_e = I_p / 4\pi d^2{% endmath %}. However, this quantity does not always work well for computer graphics as we only have a limited number of pixel intensities, but the {% math %}d^2{% endmath %} term changes too rapidly. Instead we use {% math %}I_e = I_p / (k_c + k_1 d + k_q d^2){% endmath %} - **inverse square law**, where we choose the values of {% math %}k_c, k_l{% endmath %} and {% math %}k_q{% endmath %} for best results.

Our local illumination model which takes into account both ambient and distance diffuse is as follows.
{% math %}
\large
I = k_aI_a + \Big(\frac{I_p}{k_c + k_1 d + k_q d^2} \times k_d \times (\hat{N} \cdot \hat{L})\Big)
{% endmath %}