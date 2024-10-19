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
- <mark style="background:lightpink">Dependant on scale</mark>

