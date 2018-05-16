# Region Processing

## Convolution
{% math %}
\large
g'(r,c) = \sum_{x = \infty}^\infty \sum_{y = \infty}^\infty g(r-x, c-y) \cdot t(x, y)
{% endmath %}

A small **template**, {% math %}t(x, y){% endmath %} is placed over all possible image locations and at each location, the product of an image value and the overlapping template value is computed. The products are summed to give the output value at that location. Optionally, we can subtract an offset to guarantee that the minimum output value is zero and to scale the result such that the result such that the maximum value will lie within the range allocated to an image value. The multiplicative scaling factor could be the sum of the template values.

The dimensions of the template define the practical limits of the summation. In practice, the size of the templates are usually small, from 3 x 3 to 20 x 20, at most. Convolution is used in smoothing (noise reductionn), sharpening (edge detection) and template matching (finding objects).

#### Composite Filters
As convolution is distributive, we can create a composite filter and do a single convolution.
{% math %}
\large
A \otimes (B \otimes C) = (A \otimes B) \otimes C
{% endmath %}

#### Separable Templates
For example, we can seperate the Laplacian template into two separated kernels.
{% math %}
\begin{bmatrix}
    0 & -1 & 0 \\
    -1 & 4 & -1 \\
    0 & -1 & 0
\end{bmatrix}
\; \text{can be broken down into} \;
\begin{bmatrix}
    -1 & 2 & -1
\end{bmatrix}
\; \text{and} \;
\begin{bmatrix}
    -1 \\
    2 \\
    -1
\end{bmatrix}
{% endmath %}

Instead of applying 9 operations each time, we apply 6 operations. Now consider a {% math %}n x n{% endmath %} template, we'd have to do {% math %}n^2{% endmath %} operations, which is more than {% math %}2n{% endmath %}, for {% math %}n > 2{% endmath %}.

### Smoothing
{% math %}
\large \sum (x + n) = \sum x + \sum n \approx \sum x
{% endmath %}

The aim of **smoothing** is to reduce *noise* (sharp, sudden changes in the brightness function i.e. deviation of a value from its expected value) in a image. The noise may be caused by the image capture device or smaller objects obscuring larger objects in the scene. Another type of noise is referred to as **salt and pepper** where the pixel takes either extreme values (white or black), hence the name.

#### Local Smoothing
With local smoothing, details are removed, ringing is introduced and the noise amplitude is reduced by the template length.

#### Adaptive Smoothing
``` output = s if |s - x| < T else x ```

#### Rank-Filters
**Rank-filters** are used in preference, but at a higher computational cost. A rank filter will select one value as the smoothed value from the set of pixels in a neighbourhood that have been ranked according to their magnitudes. An example of it is median filtering.

An example of this is median filtering, which is done as follows
```
Start with a fixed kernel size, for example 3 x 3
Move the origin of the kernel systematically and store the 9 values
Sort the values in the array
Pick the middle element and assign it to the output value
Repeat for every pixel in the image
```

#### Gaussian Smoothing
**Gaussian smoothing** is used to reduced ringing by using weighted smoothing, where the weights are derived from Guassian distribution.

### Sharpening
**Sharpening**, also known as edge enhancement, with the purpose of enchancing discontinuities for edge detection. It is both perceptually important and computationally important.

An **edge** is a extended, significant, local change in image intensity:
- **Significant**: the difference between two pixels in relation to their brightness, there will be a monotonic change from significant to insignificant as the proportional change in image intensity across the edge decreases
- **Local**: the difference between two adjacent pixels will be of greater significance than the same difference between two pixels separated by the images width
- **Extended**: the edge is not simply a local variation that could have arised by random fluctuations of the data

If an edge is a discontinuity, we can detect it by differencing and applying convolution with an appropriate template.

##### Roberts Cross Edge Detector
{% math %}
\begin{bmatrix}
    -1 & 0 \\
    0 & 1
\end{bmatrix}
\; \text{and} \;
\begin{bmatrix}
    0 & -1 \\
    1 & 0
\end{bmatrix}
{% endmath %}
This edge detector is very noise sensitive, if one pixel is corrupted, the edge strength is equally corrupted. It has an awkward localisation as it gives the edge strength on the joint between the four pixels.

##### Prewitt Edge Detector
{% math %}
\begin{bmatrix}
    -1 & -1 & -1 \\
    0 & 0 & 0 \\
    1 & 1 & 1
\end{bmatrix}
\; \text{and} \;
\begin{bmatrix}
    -1 & 0 & 1 \\
    -1 & 0 & 1 \\
    -1 & 0 & 1
\end{bmatrix}
{% endmath %}

##### Sobel Edge Detector
{% math %}
\begin{bmatrix}
    1 & 2 & 1 \\
    0 & 0 & 0 \\
    -1 & -2 & -1
\end{bmatrix}
\; \text{and} \;
\begin{bmatrix}
    1 & 0 & -1 \\
    2 & 0 & -2 \\
    1 & 0 & -1
\end{bmatrix}
{% endmath %}

With the **Prewitt/Sobel** edge detector, the location estimate is at the centre pixel and it is more robust against noise as we are averaging pixels either side of edge location. Also, noise magnitude is reduced by {% math %}\sqrt{3}{% endmath %} or {% math %}\sqrt{4}{% endmath %}.

{% math %}
mag = \sqrt{h^2 + v^2} \quad \text{and} \quad v = \tan^{-1}\frac{v}{h}
{% endmath %}

##### Problem
- Images are always noise corrupted and so edges are also noise corrupted which will lead to problems with detection and localisation. We can improve this by smoothing.

##### Canny / Deriche Edge Detector
His requirements for an edge detector was that edges were detected, accurate localisation and provide a single response to an edge. His solution was to convolve the image with Difference of Gaussian (DoG).

##### Optimal Edge Detector
With this approach, we differentiate **twice** instead of once as in Canny's. The result of this is that we can locate edges to subpixel accuracy and the location of edges are simply at the crossing of the zero axis.

## Template Matching
**Template matching** is a technique to measure similarities, to find things. First, we need to define a template for the model of the object to be recognised. We also need a measure of similarity between template and similar sized image region.

#### Similarity measure using convolution
We can measure **disimilarity** between image {% math %}f[i, j]{% endmath %} and {% math %}g[i, j]{% endmath %} by placing the template on the image and compare corresponding intensities. We can use measures of dissimilarity by
{% math %}
\sum_{[i, j] \in R}(f - g)^2
{% endmath %}

The limitations of this method is that we will need a template for each orientation and size of the object or partial views as the object may be partially blocked by items closer to the camera.