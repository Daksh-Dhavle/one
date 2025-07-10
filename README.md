import matplotlib.pyplot as plt
import matplotlib.animation as animation
import numpy as np

# Define two persistence diagrams
diag1 = [(0.1, 0.6), (0.2, 0.7), (0.4, 0.9)]
diag2 = [(0.15, 0.55), (0.25, 0.75), (0.85, 0.95)]  # Perturbed + one large persistence

# Interpolation function
def interpolate_diagrams(d1, d2, t):
    return [((1 - t) * x1 + t * x2, (1 - t) * y1 + t * y2) for (x1, y1), (x2, y2) in zip(d1, d2)]


# Initialize the plot
fig, ax = plt.subplots(figsize=(6, 6))
ax.set_xlim(0, 1)
ax.set_ylim(0, 1)
ax.set_aspect('equal')
ax.set_xlabel("Birth")
ax.set_ylabel("Death")
ax.set_title("Persistence Diagram Interpolation")
ax.plot([0, 1], [0, 1], 'gray', linestyle='--')  # Diagonal

# Initial scatter and lines
points1 = ax.scatter(*zip(*diag1), c='blue', label='Diagram 1')
points2 = ax.scatter([], [], c='red', label='Interpolated Diagram')
lines = [ax.plot([], [], 'k--')[0] for _ in diag1]
ax.legend()

# Animation update
def update(frame):
    t = frame / 30
    interp = interpolate_diagrams(diag1, diag2, t)
    points2.set_offsets(interp)

    for i, line in enumerate(lines):
        x_vals = [diag1[i][0], interp[i][0]]
        y_vals = [diag1[i][1], interp[i][1]]
        line.set_data(x_vals, y_vals)

    return points2, *lines

# Create animation
ani = animation.FuncAnimation(fig, update, frames=31, interval=200, blit=True)

# Save or show
plt.show()
