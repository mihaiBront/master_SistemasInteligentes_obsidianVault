---
Subject: Computer Vision
Teacher: "@FilibertoPlaBañón"
Topic: 
date: 
tags:
  - SJK002-ComputerVIsion
---
# 8.1 Interest points

![[Pasted image 20241014172635.png]]

**AIM**:
- Extracting interest points
- Characterizing them
- Equivalences on two images

Locking at the pictures below, we can reason that the most interesting interest points would be either borders (to some extent) and (especially) corners. What is clear is that inner points (areas of the shapes) are not very useful as interest points.

![[Pasted image 20241014173032.png]]

# 8.2 Corner detection

**CORNERS:** are very interesting points due to their high discriminating nature, but, how do we detect them?

- Recognizing points by making windows.
- Shifting a window in any direction should give big changes in intensity and gradient direction.

![[Pasted image 20241014173413.png]]
![[Pasted image 20241014173430.png]]

**Automating corner identification (Principal Component Analysis, *PCA*):**

1. Subtract the mean for each data point (*sort of normalization*)
	- Changes the reference coordinates space (max_variability, min_variability)
	- The center of this new reference axes is the 
1. Compute the covariance matrix
2. Compute [Eigenvectors](link) and [Eigenvalues](link) 
3. The components are the eigenvectors ranked by the eigenvalues

![[Pasted image 20241014173704.png]]
As you can see, the eigenvectors in the third case have both a large magnitude, which indicates the presence of a corner.

![[Pasted image 20241014174242.png]]

For all the windows (centered on specific pixel) that categorize as a potential corner/interest point, a value is assigned according to the magnitude and sepparation of its eigenvectors. For finding the exact corner, local minima are searched for.

# 8.2 Corner detection algorithms

## 8.2.1 Harris

For each pixel, an $H$ matrix is defined

$$
H = \sum w(x,y) \begin{bmatrix}  
I_xI_x && I_xI_y\\
I_xI_y && I_yI_y
\end{bmatrix}
$$

$$
I_x = \frac{\partial f}{\partial x},I_y = \frac{\partial f}{\partial y}
$$
![[Pasted image 20241014174827.png]]
Then, compute the eigenvalues for $H$:

$$
H = \begin{bmatrix}
a && b\\
c && d
\end{bmatrix},
\lambda \pm = \frac{1}{2}((a+d) \pm \sqrt{4bc + (a-d)^2})
$$
**SIMPLIFICATION** -> R (response) function

$$
R = det(M) - \alpha · trace(M)^2
$$

Where:

$$
det(\begin{bmatrix}
a && b \\
c && d
\end{bmatrix}) = ad - bc ,
trace (\begin{bmatrix}
a && b \\
c && d
\end{bmatrix}) = a + d
$$
(much simpler computationally)

![[Pasted image 20241014175624.png]]
![[Pasted image 20241014175635.png]]
![[Pasted image 20241014175721.png]]
![[Pasted image 20241014175735.png]]
![[Pasted image 20241014175759.png]]
N-highest value points are considered interest points

The following steps are:
- Once we detect potential interest points, we define the characteristics of any interest points, by means of a descriptors/characterizers
- Matching *IPs* in both images by that properties

**Benefits and drawbacks**
- <mark style="background:lightgreen">Invariant to translation</mark>
- <mark style="background:lightgreen">Invariant to rotation</mark>
- <mark style="background:lightpink">Dependant on scale, skew and warp</mark>

## 8.2.2 Corners. Scale invariance (DOG operator)

**REVIEW: Laplacian Operator**

![[Pasted image 20241021152759.png]]

Characterizes difference between inside and outside a target circle at a given scale, determined by a sigma ($\sigma$)

![[Pasted image 20241021152918.png]]

- Scale Characterization: Use of a Gaussian, which is invariant to scale
	$$
	f · Gauss_{\sigma} · Gauss_{\sigma'} = f · Gauss_{\sigma''} 
	$$
	Other properties (*Lindeberg, 1999*)

- In practice, the Laplacian can be approximated by a **Difference of Gaussians** (*DoG*)

![[Pasted image 20241021153336.png]]
*Figure: Example of calculating the DoG with the equivalent of a Laplacian of sigma $\sigma$*

Here, we are not only interested in the *local maxima* in the image plain, but also in the axis of different scale, in order to find interest corners.


![[Pasted image 20241021154157.png]]

1. Find local maxima in the image plain for each scale
2. In the scale axis, find maxima for a same point.
3. Weight accordingly for best interest point detection.

How does it work?

![[Pasted image 20241021154628.png]]
- Octaves are defined, for the first octave, sigmas are defined
- Sigmas are applied and checked compared 2-2
- When the Gaussian grows more than an octave, instead of doubling sigmas, we reuse previous sigmas and we downscale image (effectively, sigma applied is the double)

Then:
- Each point, on each scale, is compared to its 8 neighbors in its scale and 9 neighbors in other scales.

![[Pasted image 20241021155243.png]]

Once a point is found, in order to compare two images; say...two perspectives of the same object, we want to make a window, a *patch* around the point, and make a *descriptor*, a vector that identifies the region. Similar regions should have similar descriptors.

![[Pasted image 20241021155512.png]]
## 8.3 Patches descriptors

### Raw:

Simplest way would be to flatten the patch and make a vector of all the ordered pixels in the patch. The problem with this is that it depends on color, saturation, luminance....

### SIFT (Scale-Invariant Field Transform)

It is invariant to scale, rotation, lighting condition...

SIFT is based on information from gradients.
![[Pasted image 20241021160325.png]]
1. Perform the gradient on the patch
2. Divide in 4x4 regions (in this case, we obtain 4 regions, **but in standard SIFT case is 16x16 sized patches**)
3. For each region, perform a histogram of the directions (reducing the bins to 8, for this example, multiples of 45º)
4. That histogram represents the *Descriptor* of a region. 
5. The description is the **concatenation of the Descriptor vector for each region**

**Properties:**
- Very robust to perform matching 
- Can handle changes in viewpoint 
- Up to about 30 degree out of plane rotation 
- Can handle significant changes in illumination 
- Fast and efficient. Can run in real time

>[!example] Application: Image Stitching
>1. Identify interest points
>	![[Pasted image 20241021162058.png]]
>2. Find matching points by applying a threshold on the descriptor vectors.
>	![[Pasted image 20241021162109.png]]
>3. Apply the necessary geometric transform (WarpAffines, WarpPerspective, simple Translation...):
>	![[Pasted image 20241021162216.png]]

### Other interest point descriptors

- Daisy: circular gradient binning.
	![[Pasted image 20241021162803.png]]
- SURF: 4 bins gradient histogram
	![[Pasted image 20241021162907.png]]
- Texture features (Local Binary Pattern, *LBP*), basicamente el proceso inverso de texturizar (colocar imagenes sobre superficies) en diseño.
	
	Local binary pattern is based on binarizing the pixels around another pixel, depending if they have a bigger or lower value. This forms an 8 bit binary number. (one LBP per pixel)
	
	This is done for every pixel in a region, and a histogram is performed for that region.
	
	
	![[Pasted image 20241021162944.png]]

# 8.3 Descriptors and matching

- Matching image patches:
	Distance is calculated as the SSD:
	$$
	SSD(f_1, f_2) = \sum_{i=0}^N f_1 - f_{2, i}
	$$
	Where:
		$f_1$ is the point on image 1
		$f_2$ is the point on image 2
	
	And we also compute the ratio distance between the closest and the second closest, to avoid false matches:
	$$
	SSD_{ratio}(f_1, f_2) = \frac{SSD(f_1, f_2)}{SSD(f_1, f_2')}
	$$

# 8.4 Bag of words (bag of visual words)

![[Pasted image 20241021171457.png]]

It is a document representation, **order-less**, by frequency of appearance, from a dictionary.

![[Pasted image 20241021171843.png]]

In computer vision, we can have "visual words" characterized by textures -> Called "*textons*". For stochastic textures, it is the identity of *textons*, not their spatial arrangement, what matters.

![[Pasted image 20241021172104.png]]

![[Pasted image 20241021172226.png]]

![[Pasted image 20241021172337.png]]

Each visual word will be determined by the descriptor, analogously to how the letters of a word define it. 

For training, apply clustering to the collection of example, in order to obtain classes, which will be later associated to a label

![[Pasted image 20241021172538.png]]

