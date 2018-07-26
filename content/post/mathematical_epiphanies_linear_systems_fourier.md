---
title: "Mathematical Epiphanies 1: Linear Systems & Fourier Transform"
date: 2018-07-25
tags: ["maths", "learning", "engineering", "teaching", "fourier transform", "signals and systems"]
draft: false
---

[Click here for introduction to this series and motivation](/post/mathematical_epiphanies_introduction/).

### Linear Algebra Basics

  1. There are two ways to look at matrices.
  2. One view is about a matrix representing some data (to be covered in later part of this series)
  3. Another view is: matrix encodes the operation of a linear system on its inputs.
  4. In this (3) view, a matrix represents a linear map. Matrix-vector multiplication is taking a vector
     represented in one coordinate system and transforming it to vector in another co-ordinate systems.
  5. A diagonal matrix represents the operation of a map which only stretches (or shrinks, inverts) axis of
     original coordinate system. So linear map which results in diagonal matrices is simple.
  6. If I know what a linear map does to an axis of the input coordinate system, I can find out what it does to
     any vector in that input coordinate system (applying linear combinations).

### Linear Dynamical Systems, Fourier Transform

  1. Lot of (wo)man-made systems are signal processors. They take a signal, do something to it, and output a result.
  2. No system (except theoretical or simulated) will be purely linear in nature. However, for within practical
     working range of inputs, the system can be thought of as doing a linear operation to its inputs.
  3. A musical amplifier cannot infinitely keep bumping up the volume. It is limited by its power supply and frequency
     range of input signal. But then within reasonable limits, its action is linear.
  4. If I model such systems with an additional constraint: parameters of the system don't change with time, I get the
     classic linear time-invariant systems.
  5. A system such as in 4 can be thought of a linear map. It takes signal represented in some coordinate system,
     applies a linear map operation to it to transform it into another coordinate system.
  6. If I'm designing/synthesizing or analyzing an LTI system, I would be great for its map to be a diagonal matrix
     since it makes it simple to analyze.
  8. Another view: if I could do a transformation of my input space (A) to another coordinate space (B) such that operation
     of my system is stretching or shrinking of axes of B, it is great. Because then I can predict/quantify the behaviour of my system for any arbitrary input -> I know what happens to axes of space B. So for any arbitrary input signal represented in space B, I know what my system does.
  9. If only there was such co-ordinate space that exists and I could represent my signals in that space.
  10. It turns out that for sinusoidal signal inputs, any LTI system will act as a diagonal matrix. (Obscured by
      technical jargon which says sinusoids are eigenvectors for linear maps of LTI systems).
  11. But I'm not interested in sinusoidal inputs. I'm interested in arbitrary signal inputs like my voice going
      into an audio recorder with a background noise of my neighbour's annoying dog barking.
  12. Can I represent any arbitrary signal in coordinate space represented by sinusoids of different frequencies
      as axis? Is that representation accurate?
  13. The answers for both the above questions is no in theory. But yes in practice.
  14. If I can assume that all signals of interest to me (potential inputs to my system) follow some conditions,
      I can find a way to represent them in space of sinusoidal axis. The representation will be reasonably accurate
      (probably as accurate as you can get in the least square sense, read next section).
  15. That 'way of representation' mentioned in 14 means I have to find coefficients for each sinusoidal axis.
      The formula for finding those coefficients is given by Fourier series! Aha! That was long but worth it.
  16. To repeat the whole point: represent input signals in a coordinate space so that operation of the system
      on the signal is just stretching and shrinking of the axes.

### Things Still Hazy

I am not sure about the following, but probably that is how things are.

  1. Probably: the representation/approximation of signal found by Fourier transform close to the original signal in
     least square sense. In other words, if you cared about minimizing the average of squared error between the approximated signal and original signal, coefficients found by Fourier transform gets the job done. Minimizing
     the squared error makes some empirical sense: since power is proportional to the square of amplitude for many signals
     of practical interests (voltage), least square implies the power of approximated signal will be close to original
     signal.
  2. I can make things more complicated: rather than assuming a vector/axis of sinusoids where each data-point is
     a real number, I can think of each vector being made up of two-dimensional numbers (confusingly called complex numbers by mathematicians) which obey certain laws. I'm not sure why would anyone want to do this. But probably this allows us to approximate a broader class of signals using Fourier transforms. That is where complex sinusoids and Euler's formula shows up and mind explodes.
  3. How do we go from here to fast Fourier transform (which makes the computation in (16) of previous section efficient)?
  4. How do these things relate to other transforms like Laplace (which often shows up in control systems) and
     z-transform (which shows up in discrete dynamical systems)?


### References

To be added. Demystifying Mr Fourier's business for myself has been my quest for the last 10 years. There are too
many references to list down, some I can't remember. I'll add a few soon.
