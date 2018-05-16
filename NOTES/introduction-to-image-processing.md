# Introduction to Image Processing

## Data Sources
Anything that can generate a spatially coherent measurement of some property can be imaged. Images may be captured using a wide range of instruments, but they all share the property of being able to transform some type of energy into a visible form.

Some examples of these are electromagnetic: {% math %}\gamma{% endmath %}, x-rays, visible, microwave, radio etc. Other examples include sound and magnetic fields. As there are so many ways to capture an image, we will ignore the source, since they all ultimately deliver a digital image (a rectangular array of integer values).

## Image Resolution
There are three types of resolution: **spatial resolution** - loosely the number of pixels, **amplitude resolution** - the number of shades of grey of colours in the image and **temporal resolution** - the number of frames captured per second.

#### Spatial Resolution
The **angular resolution** can be used to specify how close an object must be for it to be resolved, or what objects can be resolved at a certain range. This quantity is equal to the ratio of the smallest resolvable object to the range, i.e. the number of degrees per pixel.

##### Nyquist's Theorem
A periodic signal can be reconstructed if the sampling interval is half the period. An object can be detected if two samples span its smallest dimension. We would need more samples if we want to recognise the object.

#### Amplitude Resolution
The **amplitude resolution** of a system is defined as the number of shades of grey that may be discerned in a monochrome image, or the number of distinct colours that may be observed in a colour image. The human visual system is only able to differentiate between around 40 shades of grey. The **amplitude resolution** in a digital system depends mainly on the signal to noise ratio in the camera.

#### Temporal Resolution
The **temporal resolution** is defined as the number of images that are captured per second. The **temporal resolution** used in movies is usually 24 to 48 FPS (frames per second).

## Colour Representation
Initially, Newton suggested that white light was composed of seven colours: red, orange, yellow, green, blue, indigo and violet. However, **Young** proposed that there were in fact three primary colours that could approximate almost all colours: red, green, blue.

In CIE **RGB colour space**, we represent a colour as {% math %}C = rR + gG + bB{% endmath %}, which is **perceptually non-linear**. If we change a colour's RGB value by a fixed amount, the perceived effect it generates is dependent on the original RGB values. It is also difficult to predict the change that will occur to a colour by changing one of its components by a certain amount. In short, equal distance in RGB colour space does not correspond to equal changes in perceived colour.

#### Other Colour Models
- YCrCb: takes into consideration intensity and colour difference, and it widely used in broadcasting. 
- HSV | IHS | HSB: hue (underlying colour of the sample), saturation (depth of the samples colour) and value (the intensity of the sample)
- Lab (CIELAB): can represent all perceivable colours and is device dependent. L represents lightness (where 0 is black and 100 is white), a represents green to magenta and b represents blue to yellow