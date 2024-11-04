---
Subject: Computer Vision
Teacher: "@FilibertoPlaBañón"
Topic: 
date: 
tags:
  - SJK002-ComputerVIsion
---
# 9.1 What is stereo vision?

![[Pasted image 20241028152455.png]]

# 9.2 Geometry of a binocular system (intrinsic parameters)
## 9.2.1 Projection geometry

![[Pasted image 20241028153448.png]]![[Pasted image 20241028153515.png]]
![[Pasted image 20241028153532.png]]
![[Pasted image 20241028153548.png]]

The objective is to represent $m$ in normalized coordinates
![[Pasted image 20241028153610.png]]
From the image coordinates, to normalized coordinates, we convert like:
![[Pasted image 20241028154025.png]]
![[Pasted image 20241028154039.png]]
## 9.2.2 MMperPX

There is also a relation between mm and pixels (at sensor level), and it is usually the same for the y and the x axis, due to the square character of the pixels, but if they weren't square, this relations could be different.

![[Pasted image 20241028154714.png]]

![[Pasted image 20241028154727.png]]

## 9.2.3 Perspective projection

It can happen that the lens and the sensor plane are note perfectly parallel to each other. This can also be corrected in a calibration process.

![[Pasted image 20241028155043.png]]
![[Pasted image 20241028155114.png]]
![[Pasted image 20241028155138.png]]
# 9.3 Projection geometry (extrinsic parameters)

In case we would like to change the coordinates to be referred to some point in space different to the center point of the camera sensor, we would have to perform a coordinates change:

![[Pasted image 20241028155253.png]]

Translation and rotation matrix $K$
![[Pasted image 20241028155309.png]]

In case we also have a perspective error, we would have to also apply that correction (we can chain matrices). This would be the full projection matrix

![[Pasted image 20241028155426.png]]

# 9.4 Camera calibrations

Types:  
- Linear regression (least squares). 
- Nn linear optimization. 
- Multiple planar patterns.

![[Pasted image 20241028160023.png]]

Known data about the experimental grid:
- Distance from point to point
- 3D coordinates of each point from the chosen reference point

We obtain: 
- 3D coordinates in the image plain

We want to obtain:
- Intrinsic parameters
- Extrinsic parameters
## 9.4.1 Linear regression

We have the following system:

![[Pasted image 20241028160357.png]]
Where:
- $u, v$ are known (3D projection)
- $X, Y, Z$ are known (physical 3D point coordinates)
- $m_{x,y}$ are the components of the projection matrix (multiplication from below)

![[Pasted image 20241028160606.png]]
![[Pasted image 20241028160546.png]]

This system of equations can be extended to whatever number of points we can take (for more precision), the generic matricial expression for that system would be:
![[Pasted image 20241028160831.png]]
This system could be solved by simply solving an equation system, but our system is **most likely not ideal**, so it has to be solved by ***least squares***.

$$Am = b$$
$$A^tAm = A^tb$$
$$m=(A^tA)^{-1}A^tb$$
>[!fail] Drawbacks
>- All camera parameters summarized in a single matrix. 
>- Can predict how any 3D point is projected into the image plane.

>[!success] Disadvantages: 
>- Does not provide specific information for each of the camera parameters. 
>- Mixture of intrinsic and extrinsic parameters: 
>	- Dependent on camera position. 
>	- If camera moves, the estimated projection camera is not the same one

# 9.5 Binocular system geometry

Cameras can be placed however we want in space, but we'll be evaluating the simplest case; ***parallel geometry***

![[Pasted image 20241028161855.png]]

Two cameras that:
- Have the same orientation in space
- Their objectives are aligned
- Have a base distance separation ($b$)
- Both are the same model (for simplification)

The reference point for our calubration system will be ***Camera 1***, the new equations being:

![[Pasted image 20241028162245.png]]

Camera 1 has no extrinsic parameters (due to simplification); while camera 2 has only a translation in space ($b$)

![[Pasted image 20241028162509.png]]

> [!tldr] Note that
> Note that, since the only difference on those matrices is the $x$ axis, which means there will only be data about the 3D position along that $x$ axis...common to all binocular systems -> Parallax

Equations for $u$ are reduced to:
![[Pasted image 20241028163118.png]]
Making possible to calculate the depth of a point.

![[Pasted image 20241028163249.png]]
>[!example] Moving a point in the direction from itself to the focal center of camera 1:
>![[Pasted image 20241028163417.png]]
>
>![[Pasted image 20241028163436.png]]

## 9.5.1 What if the cameras are not parallel?

![[Pasted image 20241028171320.png]]

1. Imagine the cameras are indeed parallel (we know the base distance ($b$))
2. We have to perform the $2^{nd}$, as well as the third camera rotation, which will not affect the y and z parameters
   ![[Pasted image 20241028171753.png]]
3. From this point, the cameras are "parallel" and we can proceed in the same way as before

![[Pasted image 20241028171950.png]]

## 9.5.2 Finding matching points
![[Pasted image 20241028172158.png]]
*Any point on one V coordinate on one camera will match the same **epipolar** coordinate on the other camera's sensor*

![[Pasted image 20241028172158.png]]
Not the same exact thing can be said about non parallel systems (but the same workaround as before can be applied)

![[Pasted image 20241028172259.png]]
Therefore, parallel geometry is easier to treat than non-parallel. "*we are sure that a point appears on the same epipolar line on both cameras but we have to find the point*"

If we are dealing with non-parallel geometry, **epipolar loines have an *epipole***, a point where they converge.

In both cases, we can obtain a *fundamental matrice* to find equivalences for epipolar lines between two sensors:

$$F^{-1} = F^T$$
$$Fm_1 = l_2$$
$$F^Tm_2 = l_1$$
$$m_2^tFm_1 = 0$$
$$m_1^tF^Tm_2 = 0$$

Fundamental matrices can be obtained from calibration parameters

Or, images can be perspective-corrected and the lines are parallel from there on.

Once the *epipole* is known, finding the same point on the other sensor is as easy as defining a window around the point on the master sensor, then finding the max correlation window on the other sensor.
