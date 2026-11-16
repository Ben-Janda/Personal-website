---
title: "Thermodynamic Uncertainty Relations"
date: "2026-11-15"
summary: "Final report of the grant project"
---

This article summarizes the results of a student faculty grant project carried out at the Faculty of Mathematics and Physics under the supervision of doc. Viktor Holubec. The project aimed to study thermodynamic uncertainty relations (TUR) and fluctuation in the non-equilibrium thermodynamics of small systems. However, in the process, it shifted focus toward numerical methods for solving Langevin stochastic differential equations with arbitrary force fields. A byproduct of the project is a Python package named `LSolver` designed specifically for this purpose, which is publicly available in my GitHub repository. [[GitHub←]](https://github.com/Ben-Janda/LSolver)

## Introduction

TUR represents a universal bound on the fluctuations of thermodynamic flows (e.g. work, heat, number of transitions or particle position). In the overdamped regime of time-homogeneous Markovian processes, these relations take the form

$$
\frac{\textrm{Var }J_t}{\langle J_t \rangle^2} \geq \frac{2k_B}{\langle \Sigma_t \rangle},
$$

where $J_t$ is the time-integrated flow over time $t$, $\textrm{Var }J_t$ is its variance, $k_B$ is the Boltzmann constant, $\Sigma_t$ is the entropy produced over the time $t$, and $\langle \bullet \rangle$ denotes the expected value. This relation imposes a universal trade-off: decreasing the flow variance (which could, for instance, increase the power reliability of a heat engine) causes an increase in entropy production (higher energy dissipation).

While TURs are already well-investigated in stationary systems, they are non-trivially violated in systems with time-dependent external driving or autonomous systems with multiple degrees of freedom. The aim of the project was to implement numerical tools for modeling these effects and to investigate the possible TUR violations by externally driven or autonomous heat engines.

## Numerics and the `LSolver` package

I developed a modular Python package, `LSolver`, for simulating stochastic systems described by Langevin equation. It solves the equation in both the underdamped regime

$$
m\ddot x(t) = -\gamma \dot x(t) + F(x,t) + \sqrt{2\gamma k_B T}\xi(t)
$$

and the overdamped regime

$$
\dot x(t) = \frac{1}{\gamma} F(x,t) + \sqrt{2D}\xi(t),
$$

where $m$ is the particle mass, $x(t)$ is the particle position, $\gamma$ is the friction coefficient, $F(x,t)$ is the external force field, $T$ is the reservoir temperature (reservoir is the liquid with the Brownian particle) and $\xi(t)$ is the Gaussian white noise with zero mean and autocorrelation $\langle \xi(t) \xi(t') \rangle = \delta(t-t')$. Overdamped regime is recovered in the limit $m \to 0$.

For the overdamped regime, the package uses the standard Euler-Maruyama discretization. For the underdamped regime, package employs a second-order Verlet-type algorithm.

More information about the package, methods, documentation, and several examples might be found in the repository. [[GitHub←]](https://github.com/Ben-Janda/LSolver).

## Selected applications

### A. Feynman ratchet and pawl

The study explored extracting useful work from the thermal motion of particles using coupled degrees of freedom. Modelling a two-dimensional system with a periodic potential $V(x,y)$ and reservoirs with distinct temperatures showed that when the temperatures are equal, only fluctuations occur and no useful work is extracted. However, for different temperatures—specifically by cooling the pawl—the system can indeed produce useful work, such as lifting a small weight.

### B. Stirling-like cycle on optical tweezers

I have simulated a microscopic heat engine represented by a Brownian particle in a harmonic potential with adjustable stiffness $k(t)$ and reservoir temperature $T(t)$:

$$
U(x,t) = \frac{1}{2}k(t) x^2.
$$

Analyzing the statstics of the stochastic trajectory ensemble verified the first law of thermodynamics: $\langle \Delta U_t \rangle = \langle W_t \rangle + \langle Q_t \rangle \approx 0$. Evaluating the TUR ratio $\langle \Sigma_t \rangle \textrm{Var } J_t / \langle J_t\rangle^2$ qualitatively proved violation, as the value was approximately $1.44 k_B < 2 k_B$ for the selected conditions of the simulation. This aligns with the hypothesis that driven non-equilibrium systems can generate fluctuations smaller than those allowed by standard TUR bounds.

## References

- *GitHub:* [Ben-Janda/LSolver](https://github.com/Ben-Janda/LSolver) - source code of the package, documentation, Jupyter notebooks, included automated tests through GitHub actions.