UKF Implementation in C#
========================

The [Unscented Kalman Filter (UKF)](http://en.wikipedia.org/wiki/Kalman_filter#Unscented_Kalman_filter) is a state
estimation filter for non-linear systems whose unknowns (noise) have gaussian distribution.

Huh?
----

You can use this code to estimate the internal state of objects by observing only some of their state.

What?
-----

Consider the accelerometer found on most phones today. The accelerometer reports the the accelerations experienced by
the phone in realtime and has a fixed margin of error. The accelerometer is usually only useful for measuring 
small and quick movements. Through some code, it can also be filtered to produce the orientation of the phone.

There is also the GPS sensor on the phone. The GPS reports the estimated position of the phone in the coordinates of
the earth. It has ever-changing margins of error that make it useful only for large movements (about 1 meter at best).

We now have two sensors that report on some subset of the physical state of the phone. 

The physics of kinetics tells us how the various elements of the physical state of an object progress are related to
each other in time. Using kinetics, we can combine these two specialized sensors to synthesize a more accurate and
complete estimate of the physical state of the phone.

We can estimate the other non-measured states of the phone - such as its velocity. 

We must, however, do this very carefully since each of the sensors has their own error margins. They need to be
combined in a statistically correct way.

And there are more sensors: gyros and magnetic compasses each with their own amazingly complicated errors and
inaccuracies. These sensors can be combined with the other two sensors to further refine the physical state
estimation.

How?
----

Let's combine those two sensors into one object called PositionSensor.

