# Region Detection and Description

A **blob** is a set of pixels that share some property and are connected (i.e. it is possible to trace a path from one member to all others). By *share some property*, we need to choose properties such that two adjacent pixels with similar properties belong to the same object.

#### Connectivity
A pixel is **4-connected** if it is connected to the four nearest neighbours (north, south, east, west) - objects joining at corners can be disconnected. A pixel is **8-connected** to the eight pixels surrounding it - which solves the corner problem but can pierce thin objects.

## Connected Component Analysis (CCA)
```
FIRST PASS
work from left to right and top to bottom
  if zero neighbours have a label
    pixel receives the next free label
  if one or more neighbours have the same label
    pixel receives the same label
  if two or more neighbours have different labels
    pixel recieves one label, equivalence is recorded

SECOND PASS
work from left to right and top to bottom
relabel all equivalent labels
```

## Blob Description
#### Moments of Area
{% math %}
M_{\alpha\beta} = \sum_{image}x^\alpha y^\beta f(x, y)
{% endmath %}
If the image {% math %}f(x, y){% endmath %} was binary, then:
- {% math %}M_{00}{% endmath %} gives the sum of the pixels
- {% math %}M_{10}{% endmath %} gives the sum of x values of the regions pixels
- {% math %}M_{01}{% endmath %} gives the sum of y values of the regions pixels
- {% math %}(M_{10} / M_{00}, M_{01} / M_{00}){% endmath %} gives the regions centre of gravity

#### Central Moments of Area
{% math %}
M_{\alpha\beta} = \sum_{image}(x - \bar{x})^\alpha (y - \bar{y})^\beta f(x, y)
{% endmath %}
where {% math %}(\hat{x}, \hat{y}){% endmath %} is the centre of gravity. Now, since this depends on the centre of gravity, the position of the region on the image doesn't affect the central moments. However, orientation will affect the central moments of area but this can be useful if we want to work out the orientation of an object.

Moments can be defined for all values of. There is a limit to the number of the useful ones - so we can use the lower order moments to make higher order ones invariant to position, orientation and size of region. We can compute these for non-binary images too and we can modify the computation to use labelled blobs. The values of these moments can be used to discriminate blobs based on size / shape.

## Chain Codes
**Chain codes** are used to trace the outline of blobs - following the pixels on the boundary, keeping the interior of the object to one side. Chain code is the direction of the jump to the next pixel and this description is position independent and orientation dependent. It is possible to use differential chain codes so that 0 is the direction of movement from the previous pixel - and an important property is rotational invariance.

#### Perimeter
From regular chain codes, even codes have length 1 and odd codes have length {% math %}\sqrt{2}{% endmath %} thus the perimeter length is {% math %}even + \sqrt{2} * odd{% endmath %}, where the unit is the length of a pixel side.

#### Area
| Code | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| ---- | - | - | - | - | - | - | - | - |
| Area | 0 | h + 0.5 | h | h - 0.5 | 0 | -h - 0.5 | -h | -h + 0.5 |

The major disadvantage of this is that if the image is affected by noise, as it will be, then this will be at the pixel level and the chain codes will be directly affected. This difficulty can be avoided if we replace approximately linear segments of the boundary by straight line segments. The boundary of an object is thereby reduced to a polygon. This is achieved in two stages: In the first stage, sections of the boundary are divided into linear segments. In the second stage adjacent segments are merged, if necessary.

#### Colour Distribution
**Colour distribution** is a useful characteristic of blobs, that is independent of area and orientation. Typically we record the hue and saturation components and normalise out the brightness. This is useful for when we want to track peoples movement.

#### Blob Tracking
The problem we have is how to be match a blob in one image with another blob in another image. We have to look for invariant properties - those that will be the same in both images. Difficulties we might face is that there may be alot of blobs or that blob population might change.

#### Predictive Tracking
In predictive tracking, for each blob, we store the current location, current velocity and invariants about each blob. We then predict the location of this blob in the next frame and we verify that the blob is near the predicted location and that the predicted blob has the same invariants as the new blob.

Problems with this approach is that multiple blobs may collude with one another, but they may split in the future so we need to take care of this. What if a blob is not at a predicted location - we would need to keep track of the missing blob for a while.