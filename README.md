# Rigid Body Dynamics

This project implements a rigid body dynamics solver based on quaternions.  
The equations of motion are formulated in the body-fixed reference frame. 
The solver is written in Fortran (gfortran compiler is required)

---

## Overview

The solver computes the rotational and translational motion of a rigid body subject to external torques and forces.  
Quaternions are used instead of Euler angles to avoid singularities (gimbal lock) and ensure numerical stability.

---

## Reference Frames

- **Inertial frame**: fixed reference frame
- **Body-fixed frame**: attached to the rigid body and rotating with it

All dynamic equations are expressed in the body-fixed frame.

---

## State Variables

The system state is defined as:

$$
\mathbf{s} =
\begin{bmatrix}
\mathbf{x} \\
\mathbf{v} \\
\mathbf{q} \\
\boldsymbol{\omega}
\end{bmatrix}
$$

where:
- \ (\mathbf{x} = [x, y, z]^T \)\ is the position of the center of mass
- \ (\mathbf{v} = [u, v, w]^T \)\ is the velocity of the center of mass
- \( \mathbf{q} = [q_0, q_1, q_2, q_3]^T \)\ is the unit quaternion representing the orientation of the body
- \( \boldsymbol{\omega} = [\omega_x, \omega_y, \omega_z]^T \)\ is the angular velocity vector in the body-fixed frame

---

## Equations of Motion


The system evolves according to the following set of ODEs:

$$
\frac{d\mathbf{s}}{dt} = 
\frac{d}{dt}
\begin{bmatrix}
\mathbf{x} \\
\mathbf{v} \\
\mathbf{q} \\
\boldsymbol{\omega}
\end{bmatrix} = 
\begin{bmatrix}
\mathbf{v} \\
\mathbf{F}/m \\
\frac12 G^T \boldsymbol{\omega} \\
I^{-1}[\mathbf{M} - \boldsymbol{\omega} \times I \boldsymbol{\omega}]
\end{bmatrix}
$$

---


## Numerical Integration

The equations are integrated in time using a first order Euler integrator (better choices will be available soon!).  
Quaternion normalization is enforced to preserve unit norm:

$$
\mathbf{q} \leftarrow \frac{\mathbf{q}}{\|\mathbf{q}\|}
$$

---
