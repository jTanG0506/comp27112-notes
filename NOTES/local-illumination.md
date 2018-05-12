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