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

### Modelling specular reflection
{% math %}\hat{R}{% endmath %} is a vector giving the direction of maximum specular reflection, which makes an angle of {% math %}\theta{% endmath %} with {% math %}\hat{N}{% endmath %}, as does {% math %}\hat{L}{% endmath %}. {% math %}\hat{V}{% endmath %} is a vector pointing to the observer's position.

The specular reflection varies with:
- the angle {% math %}\phi{% endmath %} between {% math %}\hat{R}{% endmath %} and {% math %}\hat{V}{% endmath %}

- incident angle {% math %}\theta{% endmath %} and light wavelength {% math %}\lambda{% endmath %}

So we need a function {% math %}I_{specular} = S(\phi, \theta, \lambda){% endmath %}

#### The effect of {% math %}\phi{% endmath %}
As {% math %}\hat{V}{% endmath %} diverges from {% math %}\hat{R}{% endmath %} by angle {% math %}\theta{% endmath %}, the viewer sees less specular reflection. We need a function {% math %}F(\phi){% endmath %} which models the variation of observed specular, and Bui-Tuong Phong proposed using the function {% math %}F(\phi) = cos^n \phi{% endmath %}. So now we have {% math %}I_{specular} = I_p\cos^n\phi = I_p(\hat{R} \cdot \hat{V})^n{% endmath %}.

#### The effect of {% math %}\theta \, \text{and} \, \lambda{% endmath %}
Augustin-Jean Fresnel, the founder of the wave theory of light, developed the theory of diffraction of light. The complex variation is expressed by the Fresnel equation.

{% math %}
\large
F = \frac{1}{2}\Big[\frac{\sin^2(\phi - \theta)}{\sin^2(\phi + \theta)} + \frac{\tan^2(\phi - \theta)}{\tan^2(\phi + \theta)}\Big]
{% endmath %}

where F is the fraction of light reflected. We have that {% math %}\sin \theta = \sin \phi / \mu{% endmath %} where {% math %}\mu{% endmath %} is the refractive index of the material, independent of {% math %}\lambda{% endmath %}. However, in practice, we often replace {% math %}F{% endmath %} with a single constant {% math %}k_s{% endmath %}, the **specular reflection coefficient** of the surface, with {% math %}0 \leq k_s \leq 1{% endmath %}. So now we have {% math %}I_{specular} = I_pk_s(R \cdot V)^n{% endmath %}. We have sacrificed accuracy for efficiency, which results in a plastic look.

Our local illumination model which takes into account ambient, distance diffuse and distance specular is as follows.

{% math %}
\large
I = k_aI_a + \frac{I_p}{k_c + k_1 d + k_q d^2}\Big(k_d(\hat{N} \cdot \hat{L}) + k_s(\hat{R} \cdot \hat{V})^n\Big)
{% endmath %}

### Incorporating colour
It is easy to incorporate colour, as we just express light colour as a triple of RGB intensities: {% math %}I_{pR}, I_{pG}, I_{pB}{% endmath %}. Likewise, we express surface colour using {% math %}k_{aR}, k_{aG}, k_{aB}{% endmath %} for ambient and {% math %}k_{dR}, k_{dG}, k_{dB}{% endmath %} for diffuse.

For example, our local illumination model with red taken into account is as follows.
{% math %}
\large
I_R = k_{aR}I_{aR} + \frac{I_{pR}}{k_c + k_1 d + k_q d^2}\Big(k_{dR}(\hat{N} \cdot \hat{L}) + k_s(\hat{R} \cdot \hat{V})^n\Big)
{% endmath %}

### Multiple lights
If we have multiple lights, we simply compute illumination separately for each and sum. For {% math %}M{% endmath %} lights, we have:

{% math %}
\large
I = \text{ambient} + \sum^M_{i=1}(\text{diffuse}_i + \text{specular}_i)
{% endmath %}