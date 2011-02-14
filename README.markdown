UKF Implementation in C#
========================

The [Unscented Kalman Filter (UKF)](http://en.wikipedia.org/wiki/Kalman_filter#Unscented_Kalman_filter) is a state
estimation filter for non-linear systems whose unknowns (noise) have gaussian distribution.

Huh?
----

You can use this code to estimate the internal state of objects by observing only some of their state.

---

Example: Combining multiple sensors to accurately estimate the movement of objects
--------

Consider the accelerometer found on most phones today. The accelerometer reports the the accelerations experienced by
the phone in realtime and has a fixed margin of error. The accelerometer is usually only useful for measuring 
small and quick movements. Through some code, it can also be filtered to produce the orientation of the phone.

There is also the GPS sensor on the phone. The GPS reports the estimated position of the phone in the coordinates of
the earth. It has ever-changing margins of error that make it useful only for large movements (about 1 meter at best).

We have two sensors that report on some subset of the physical state of the phone. 

Kinetics tells us how the various elements of the physical state of an object are related to each other and how they
progress in time. Using kinetics, we can combine these two specialized sensors to synthesize a more accurate and
complete estimate of the physical state of the phone. We can also estimate the other non-measured states of the phone
- such as its velocity.

We must, however, do this very carefully because each of the sensors has its own error margins. These sensors
must therefore be combined in a statistically correct way.

And there are more sensors: gyros and magnetic compasses each with their own amazingly complicated errors and
inaccuracies. These sensors can be combined with the other two sensors to further refine the physical state
estimation.

Let's begin by creating an object called the PhysicalState. It contains a value for each element of the physical
state that we want to know about. Let us estimate 12 states: 3 states for the cartesian (x, y, z)
position, 3 states for the orientation of the object, 3 states for its velocity, and 3 states for its acceleration.
We can write this as:

    class Vector {
        public double X, Y, Z;
    }

    class PhysicalState {
        public Vector Position;
        public Vector Orientation;
        public Vector Velocity;
        public Vector Acceleration;
    }

We now need to model the kinetics of this state. The estimator will utilize this model in order to refine
its estimations. We do this by implementing an "Update" function that advances that state in time.

    class PhysicalState ...
        public void Update(double dt) {
            Velocity += Acceleration * dt;
            Position += Velocity * dt;
        }
        
    class Vector ...
        public Vector operator + (Vector b) {
            return new Vector() { X = X + b.X, Y = Y + b.Y, Z = Z + b.Z };
        }
        public Vector operator * (double b) {
            return new Vector() { X = X * b, Y = Y * b, Z = Z * b };
        }

Now that we have described the physics, we now need to describe how each of the sensors relates to this model.
To do this, we implement the "Observe" function.

    class PhysicalState ...
    
        public void Observe() {
            
        }

