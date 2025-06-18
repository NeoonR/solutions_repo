# Problem 1
Projectile motion, while seemingly simple, offers a rich playground for exploring fundamental principles of physics. This investigation focuses on analyzing how the **horizontal range** of a projectile depends on the **angle of projection**.

Beneath the simplicity lies a robust system governed by both linear and quadratic relationships. These make it possible to explain a broad variety of real-world trajectories — from the arc of a football to the flight of a missile.

## Theoretical Foundation

A projectile launched at an initial velocity $v_0$ and angle $\theta$ follows the path:

\[
\begin{aligned}
x(t) &= v_0 \cos(\theta) t \\
y(t) &= v_0 \sin(\theta) t - \frac{1}{2}gt^2
\end{aligned}
\]

Solving for the total time of flight $T$ when $y(T) = 0$ yields:

\[
T = \frac{2v_0\sin(\theta)}{g}
\]

The **range** $R$ is then:

\[
R = v_0 \cos(\theta) \cdot T = \frac{v_0^2 \sin(2\theta)}{g}
\]

This equation assumes the projectile lands at the same height from which it was launched.

## Python Simulation Code

```python
import numpy as np
import matplotlib.pyplot as plt

g = 9.81  # gravity (m/s^2)

def compute_range(v0, angle_deg):
    angle_rad = np.radians(angle_deg)
    return (v0**2 * np.sin(2 * angle_rad)) / g

def plot_range_vs_angle(v0_list):
    angles = np.linspace(0, 90, 500)
    plt.figure(figsize=(10, 6))
    for v0 in v0_list:
        ranges = compute_range(v0, angles)
        plt.plot(angles, ranges, label=f"v0 = {v0} m/s")
    plt.title("Range vs Angle of Projection")
    plt.xlabel("Angle (degrees)")
    plt.ylabel("Range (m)")
    plt.legend()
    plt.grid(True)
    plt.show()

v0_list = [10, 20, 30, 40]
plot_range_vs_angle(v0_list)
```

## Analysis of the Range

- The **maximum range** occurs at $\theta = 45^\circ$.
- The range graph is **symmetric** around 45°.
- A higher initial speed $v_0$ increases the range quadratically.

## Extending the Model: Launch Height

If a projectile is launched from a height $h$, the time of flight changes:

\[
t = \frac{v_0 \sin(\theta) + \sqrt{(v_0 \sin(\theta))^2 + 2gh}}{g}
\]

Thus, the updated range becomes:

\[
R = v_0 \cos(\theta) \cdot t
\]

```python
def compute_range_with_height(v0, angle_deg, h):
    angle_rad = np.radians(angle_deg)
    vx = v0 * np.cos(angle_rad)
    vy = v0 * np.sin(angle_rad)
    t_flight = (vy + np.sqrt(vy**2 + 2 * g * h)) / g
    return vx * t_flight

def plot_range_with_height(v0, h):
    angles = np.linspace(0, 90, 500)
    ranges = [compute_range_with_height(v0, angle, h) for angle in angles]
    plt.figure(figsize=(10, 6))
    plt.plot(angles, ranges, label=f"h = {h} m")
    plt.title(f"Range vs Angle of Projection (v0 = {v0} m/s, h = {h} m)")
    plt.xlabel("Angle (degrees)")
    plt.ylabel("Range (m)")
    plt.legend()
    plt.grid(True)
    plt.show()

plot_range_with_height(v0=30, h=5)
```

## Practical Applications

- **Sports:** Modeling ball trajectories (e.g., soccer, basketball, golf).
- **Engineering:** Optimizing launch angles for projectiles, rockets.
- **Astrophysics:** Planetary motion, asteroid impact predictions.

## Limitations and Realism

This idealized model does **not** include:

- Air resistance (drag)
- Wind
- Terrain curvature or elevation

These can be introduced through more complex numerical simulations and solving non-linear differential equations.

## Conclusion

This analysis shows that projectile range is strongly governed by the angle of projection and initial velocity. By varying these, we can simulate and optimize real-world scenarios in sports, engineering, and physics.

---

**To Do Next:**
- Add air resistance models
- Use Python libraries (e.g., SciPy) for differential equation solutions
- Deploy the script as a static visualization tool (e.g., using Plotly or Dash)
