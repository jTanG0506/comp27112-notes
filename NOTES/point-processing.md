# Point Processing

## Grey Value Transforms
**Grey value transforms** will manipulate the value of an individual pixel without considering its neighbouring values. A simple method of manipulating a greyscale of the image is to add a constant to all values, this is known as **brightness adjustment**. Another method would be to multiply everything by a constant multiplier, known as **contrast adjustment**.

## Image histogram
A **grey value histogram** records the frequency of the occurrence of each grey in the image, whereas a **colour value histogram** will be a *cube*, where each *block* will have a value of how many times an RGB triple appears in the image.

## Thresholding
We can use **thresholding** to convert a grey or colour image into a binary image. The threshold value used should aim to reduce misclassification error (assuming the two output values correspond to something sensible in the image). There are many ways to choose the threshold value, p-tile, mode, or even manually.

```
  if I(x, y) > T then I(x, y) = 1 else I(x, y) = 0
```

#### P-Tile
Say we want to extract an object from an image and we know the proportion of the image that is the object, we threshold the image to select this proportion of the pixels from the histogram.

#### Mode
We threshold at the minimum between the histogram's peaks. So we find the two modes of the distributions of the two objects, and we look for a local minimum between these two modes.

#### Automated Methods
We try to find a threshold {% math %}\theta = (\mu_1 + \mu_2)/2{% endmath %} where {% math %}\mu_1{% endmath %} is the average of the values below the threshold and {% math %}\mu_2{% endmath %} is the average of the values above the threshold.

#### Optimal Classifier
We can estimate the mean and variance of populations so then we can select our threshold T to minimise the misclassification.

## Image Zooming
**Reducing**: new value is weighted sum of nearest neighbours or it equals the nearest neighbour

**Enlarging**: new value is weighted sum of nearest neighbours and we add noise to obscure pixelation

## Geometric Transformations
### Affine Transformations
Affine transformations are those that preserve length and the area, but not necessarily the angles. Examples of affine transformations include scaling, shearing, rotation and translation.

#### Scaling
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    a & 0 & 0 \\
    0 & e & 0 \\
    0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y \\
    1
\end{bmatrix}
{% endmath %}

#### Shearing
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    1 & b & 0 \\
    d & 1 & 0 \\
    0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y \\
    1
\end{bmatrix}
{% endmath %}

#### Rotation
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    a & b & 0 \\
    d & e & 0 \\
    0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y \\
    1
\end{bmatrix}
{% endmath %}

#### Translation
{% math %}
\begin{bmatrix}
    x' \\
    y' \\
    1
\end{bmatrix}
=
\begin{bmatrix}
    1 & 0 & c \\
    0 & 1 & f \\
    0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y \\
    1
\end{bmatrix}
{% endmath %}

#### Camera Lens Distorting
Camera lens distortions introduce a degree of warping to the data. **Pinchusion distortion** results in the side of the image being distorted less than the corners, whilst **barrel distortion** results in the sides of the image being distorted more than the corners.

#### Image resampling
**Image resampling** the process of *moving* source pixels to destination pixels. may result in pixels being transformed to a non-integer and we can either truncate or round the coordinate values, which can create holes in the image. The solution is to apply the transformation in reverse. In this case, we look at an output pixel and use the reverse transformation to determine where it came from in the input image. The source may still be non-integer, but we are able to guarentee that all output pixels will be populated and so we can interpolate nearest neighbours to be the outputs pixel value.