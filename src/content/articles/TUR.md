---
title: "Thermodynamic Uncertainty Relations"
date: "2026-11-15"
summary: "Final report of the grant project"
---

This article is a summary of results of student faculty grant project solved at Faculty of Mathematics and Physics under the supervision of doc. Viktor Holubec. Project was aimed to study the thermodynamic uncertainty relations (TUR) and fluctuation in non-equilibrium thermodynamics of small systems, but in the process it became more concerned with the numerical methods of solving Langevin stochastic differential equation with arbitrary force fields. Side product of the project was a Python package `LSolver` with this exact goal, which is publicly available at my GitHub repository. [[GitHub←]](https://github.com/Ben-Janda/LSolver)

## Introduction

TUR represents universal bound for fluctuations of thermodynamic flows (e.g. work, heat, number of transition or particle position). In the overdamped regime of time homogeneous markovian processes these relations are

$$
\frac{\textrm{Var }J_t}{\langle J_t \rangle^2} \geq \frac{2k_B}{\langle \Sigma_t \rangle},
$$

where $J_t$ is the time integrated flow over time $t$, $\textrm{Var }J_t$ is its variance, $k_B$ Boltzmann constant, $\Sigma_t$ is entropy produced over the time $t$ and $\langle \bullet \rangle$ denotes mean value. From this relation stems the universal trade-off: decreasing the variance of the flow (which might be for example increasing the reliability of heat engine power) cases increase of entropy production (higher energy dissipation).

In the case of stationary systems are TUR already well investigated, but in the case of time-dependent external driving or autonomous systems with more degrees of freedom are TUR non-trivially defied. Aim of the project was to implement the numerical tools for modeling these effects and investigate the transition between explicitely driven and autonomous heat engines.

## Numerics and the `LSolver` package

I have developed a modular Python package `LSolver` for the simulations of stochastic systems described by Langevin equation. It solves the equation both in underdamped regime

$$
m\ddot x(t) = -\gamma \dot x(t) + F(x,t) + \sqrt{2\gamma k_B T}\xi(t)
$$

and in overdamped regime

$$
\dot x(t) = \frac{1}{\gamma} F(x,t) + \sqrt{2D}\xi(t),
$$

where $m$ is the particle mass, $x(t)$ particle position, $\gamma$ friction coefficient, $F(x,t)$ external force field, $T$ temperature of the reservoir and $\xi(t)$ the gaussian white noise with zero mean value and autocorrelation $\langle \xi(t) \xi(t') \rangle = \delta(t-t')$. Overdamped regime is just the limit of $m \to \infty$.

For the overdamped regime, package uses the standard Euler-Murayama discretization. For the underdamped regime, package uses the Verlet-type algorithm of second order.

More information about the package, methods, documentation and several examples might be found through the README.md at [[GitHub←]](https://github.com/Ben-Janda/LSolver).

## Selected applications

### A. Feynman ratchet and pawl

Study of extracting useful work from heat motion of particles using coupled degrees of freedom. Modelling of twodimensional system with periodic potential $V(x,y)$ and reservoirs with distinct temperatures showed that if the temperatures are same, there are only fluctuations and no useful work, but for different temperatures by cooling the pawl, the system in fact can produce useful work, for example by pulling up small weight.

### B. Stirling-like cykle on optical tweezers

Simulation of microscopic heat engine, which is represented by brownian particle in harmonic potential with adjustable stiffness $k(t)$ and temperature of the reservoir $T(t)$:

$$
U(x,t) = \frac{1}{2}k(t) x^2.
$$

Analyzing the statstics of stochastic trajectory ensemble verified the first thermodynamics law $\langle \Delta U_t \rangle = \langle W_t \rangle + \langle Q_t \rangle \approx 0$. Evaluating the fraction $\langle \Sigma_t \rangle \textrm{Var } J_t / \langle J_t\rangle^2$ present in TUR qualitatively proved violation, since the value was approximately $1.44 k_B < 2 k_B$. This corresponds with the hypothesis that driven non-equilibrium systems might create flow with variations that are not allowed by TUR.

## References

- *GitHub:* [Ben-Janda/LSolver](https://github.com/Ben-Janda/LSolver) - source code of the package, documentation, Jupyter notebooks, automated tests