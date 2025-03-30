# Problem 1
Derivation of the Governing Equations

The motion of a projectile follows Newton's Second Law, which states:

For a projectile moving under the influence of gravity alone (neglecting air resistance), the forces acting on the body are:

A horizontal component where no force acts (assuming no air resistance):


A vertical component where gravity acts downward:


Solving these equations, we obtain the motion equations:


where:

 is the initial velocity,

 is the angle of projection,

 is the gravitational acceleration.

Family of Solutions

By varying initial conditions ( and ), a family of solutions emerges. Different values of  lead to different maximum heights and ranges, while varying  alters the shape of the trajectory.

Analysis of the Range

Horizontal Range as a Function of Angle

The horizontal range  is obtained by setting  and solving for :

This function is maximized when , leading to:


Influence of Other Parameters

Initial Velocity (): Increasing  results in a larger range since .

Gravitational Acceleration (): A higher  reduces the range, as .

Angle of Projection (): The range follows a symmetric pattern around , reaching its peak at this angle.

Practical Applications

Real-World Adaptations

Uneven Terrain: By modifying the ground level equation, the projectile motion can be adapted for varied landing heights.

Air Resistance: Introducing a drag force term  alters the equations significantly, leading to non-trivial solutions.

Wind Effects: Horizontal wind introduces an additional force component, affecting trajectory curvature.

Implementation

Computational Simulation

A Python script is developed to visualize projectile motion and analyze the range as a function.
import numpy as np
import matplotlib.pyplot as plt

def projectile_range(v0, theta, g=9.81):
    theta_rad = np.radians(theta)
    return (v0**2 * np.sin(2*theta_rad)) / g

theta_values = np.linspace(0, 90, 100)
v0 = 20  # Example initial velocity in m/s
ranges = [projectile_range(v0, theta) for theta in theta_values]

plt.figure(figsize=(8,5))
plt.plot(theta_values, ranges, label=f'v0={v0} m/s')
plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (meters)')
plt.title('Projectile Range vs. Angle of Projection')
plt.legend()
plt.grid()
plt.show()
