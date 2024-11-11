---
Subject: Computer Vision
Teacher: "@FilibertoPlaBañón"
Topic: 
date: 2024-11-11
tags:
  - SJK002-ComputerVIsion
---
# 9(2).1 Structured light systems

![[Pasted image 20241111151156.png]]

Systems that consists in:
- Projector **project known patterns** onto the surfaces
- Camera that **captates** the image and interprets the known pattern to find **matching points**

Those two cooperate to make a stereoscopic system.

![[Pasted image 20241111152151.png]]![[Pasted image 20241111152204.png]]![[Pasted image 20241111152217.png]]

As for system calibration:
![[Pasted image 20241111152240.png]]

Computation of the correspondance:
![[Pasted image 20241111152305.png]]

Advantages and drawbacks:
![[Pasted image 20241111152332.png]]

# 9(2).2 Time of Flight sensors

One part of the sensor is an **emitter**, which produces a pulsated light over a certain frequency, which can either be:
- Pulsated (real pulses, square wave)
- Modulating the amplitude; continuous

The **camera** part calculates the time it took for the light (with that one specific frequency of the emitter) to get back to the sensor, from which it calculates the distance on each pixel.

$$
d=\frac{1}{2}c*\tau
$$
Where:
- $d$ is the distance from the sensor to the object
- $c$ is the speed of the light
- $\tau$ is the phase delay (in seconds)

![[Pasted image 20241111152720.png]]

## Continuous light modulation emitters

![[Pasted image 20241111153031.png]]

The variation in phase from the emitted light (green) to the relieved light (red), which can also be attenuated.

A **drawback** with this operation mode is that if the distance coincides with a multiple of the period (the phase delay is more than $2\pi$), the distance cannot longer be measured. [TOF Wrapping](https://medium.com/chronoptics-time-of-flight/phase-wrapping-and-its-solution-in-time-of-flight-depth-sensing-493aa8b21c42)

There are unwrapping methods for that:
- Emit two distinct frequencies that are not multiple of each other)
	![[Pasted image 20241111153854.png]]
- The individual phase measured on both periods will not match in some amount of periods, which allows unwrapping.

Emitted signal 
$$s(t) = a·cos(2\pi ft)$$

Received signal 
$$s(t) = A·cos(2\pi f (t-\tau)) + B$$

Cross correlation between emitted and received:
$$C(x) = lim_{t->\infty}\frac{1}{T}\int_{-T/2}^{T/2}r(t)s(t+x)dt$$
$$C(x) = \frac{aA}{2}cos(2\pi f\tau + 2\pi fx) + B$$

Light is then sampled by the sensor with the most acurate frequency possible (an assembly of integration freqs similar to how an incremental encoder works):

![[Pasted image 20241111154818.png]]
![[Pasted image 20241111154829.png]]

![[Pasted image 20241111154841.png]]

In order to measure the carrying frequency the most accurate possible (comply with *Niquist* theorem at least)

## Pulsated light modulation:

![[Pasted image 20241111155302.png]]
![[Pasted image 20241111155637.png]]

Works by synchronizing the integration time with the pulse, along with the pulse emission time.

The longer the delay, the lower the value of that integration (linearly dependent if not attenuated)

To remove that dependency over the attenuation, every second integration is granted to be made over the whole returned pulse. That way, $V_{f1} / V_{f2}$ would be """constant""" for the same phase delay for any attenuation.

![[Pasted image 20241111160011.png]]

A big drawback is a maximum distance (similar to how the multiples of the carrying emitter freq was an issue with continuous signal modulation)

In actuality, two clocks are used for integration in pulsated modulation:
![[Pasted image 20241111161106.png]]

As for the **precision**:
![[Pasted image 20241111161135.png]]

## General considerations:

- The main **source of error** in this kinds of sensors is due to light attenuation.
  