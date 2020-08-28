---
title: Notes for Computer Vision
date: 2020-02-12 02:14:17
author: FadingWinds
img:
categories: Machine Learning | 机器学习
tags:
    - notes
    - CT courses
    - updating
summary: Personal notes for computer vision.
top:
hidden:
password:
---
### Basic Concepts

**Image**: a grid(matrix) of intensity values

**Kernel**: the filter/mask.

**Gradient**(of an image): $▽f = [df/dx, df/dy]$(partial derivative), points in the direction of the most rapid increase in intensity.


### Filter

Form a new image whose pixels are a combination of the original ones.

Use filter to get useful information from images or enhance the image.

#### Linear Filter

Replace each pixel by a linear combination (weighted sums) of its neighbors.

1. Cross-correlation

    Like a dot product - sum of the neighbor matrix multiplied by kernel for every exactly matching position. e.g. (5, 3) * (5, 3)


2. Convolution

    Like an outer product - compared with cross correlation, the kernel is flipped both horizontally and vertically.

    Convolution is commutative and associative. 

    Why convolution: consider the situation between two identical images except one of them is flipped/turned/etc.

3. Blurring / Sharpening

    Mean filter, box filter

#### Gaussian filter
    
Apply a Gaussian function. Recall that the weights should always be normalized to sum=1.

Removes "high-frequency" components (low-pass filter).

Convolution with itself will get another Gaussian filter.



### Edge Detection

Convert a 2D image into a set of curves.

An edge is a place of rapid change in the image intensity function.

Edges are caused by a variety of factors:

- surface normal discontinuity
- depth discontinuity
- surface color discontinuity
- illumination discontinuity
  
To differentiate a digital image:

- reconstruct a continuous image, then compute the derivative
- take discrete derivative(find difference)

For noisy input images, smooth them first with Gaussian filter(?).

**Sobel Operator**: Common approximation of derivative of Gaussian (need the 1/8 to get the right gradient magnitude)

Thresholding edges - 2 thresholds, 3 cases (strong edge)

Connecting edges - Weak edges are edges if they are connected to strong edges. Look in some neighborhood (usually 8 closest).

#### Canny Edge Detector

1. Filter image with derivative of Gaussian
2. Find magnitude and orientation of gradient
3. Non-maximum suppression
4. Linking and thresholding (hysteresis): one low and one high threshold; use the high one to start edge curves and the low threshold to continue them

Parameters: high threshold, low threshold, sigma(width of the Gaussian blur, large sigma detects large-scale edges)



### Sampling

#### Down-sampling

Making smaller images.

e.g. throw away every other row and column to create a 1/2 size image.

**Aliasing**: Get a wrong image by sub-sampling (this problem occurs especially for synthetic images).

Aliasing occurs when your sampling rate is not high enough to capture the amount of detail in your image.

To avoid aliasing: sampling rate ≥ 2 * max frequency in the image

**Nyquist Rate**: minimum sampling rate

Wagon-wheel effect: in video wheel appear to rotate backwards

Fix aliasing: **Gaussian pre-filtering**

**Gaussian pyramid**: Represent N*N image as a pyramid of $1*1, 2*2, 4*4, 2^k*2^k$ (assuming $N=2^k$)

#### Up-sampling / Image Interpolation

Correspondingly, making bigger images.

1. Nearest-neighbor interpolation (use neighbor value to replace)
2. Linear interpolation (use a line to fit the gap)
3. Gaussian reconstruction
4. Bicubic interpolation

### Feature Detection

Can be used to conduct automatic panoramas.

Combine two images:

1. Extract features
2. Match features
3. Align images
   
Find features that are invariant to transformations:

- Geometric invariance: translation, rotation, scale
- Photometric invariance: brightness, exposure, etc.

**Local Features** refer to a pattern or distinct structure found in an image, such as a point, edge, or small image patch.

Advantages of local features:

- Locality: robust to occlusion (means some sort of blocking of an object[not sure]) and clutter (lots of objects in the image).
- Quantity: hundreds or thousands in a single image
- Distinctiveness: differentiate a large database of objects
- Efficiency: real-time performance achievable

Good features - look for image regions that are "unusual".

#### Harris Corner Detection

Consider shifting the window $W$ by $(u,v)$, compute the squared differences (SSD). Look for high SSDs. 

$E(u,v) \approx [ u \space v ] \begin{bmatrix}A & B \\ B & C\end{bmatrix} \begin{bmatrix} u \\ v \end{bmatrix}$

$A = \Sigma I_x^2(dI/dx), B = \Sigma I_xI_y, C = \Sigma I_y^2(dI/dy)$

$E(u,v)$ is locally approximated as a quadratic error function.

Corner detection summary:

1. Compute the gradient at each point in the image.
2. Compute the "ABBC" matrix from the entries in the gradient.
3. Compute the eigenvalues.
4. Find points with large response ($\lambda_{max}>threshold$).
5. Choose these points where $\lambda_{min}$ is a local maximum as features.

In practice, using a simple window $W$ doesn't work too well. Instead, we'll weight each derivative value based on its distance from the center pixel.

#### Image Transformations

Geometric: Rotation, Scale

Photometric: Intensity

We want corner locations to be invariant to photometric transformations and covariant to geometric transformations.

**Invariance**: Image is transformed and corner locations do not change.

**Covariance**:  If we have two transformed versions of the same image, features should be detected in corresponding locations.

In *Harris Detector*:

- Corner location is covariant w.r.t. translation.
- Corner location is covariant w.r.t. rotation.
- Partially invariant to affine intensity change.
- Not invariant to scaling.

**Laplacian of Gaussian** (LoG): Use a Gaussian filter first then Laplacian.

**Difference of Gaussian** (DoG): A Gaussian minus a slightly smaller Gaussian.









