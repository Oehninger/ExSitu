#Numerical Simulations for the Impulsive Metapopulation Model

This repository contains the Python code used to generate the numerical simulations presented in the manuscript.

The simulations illustrate the dynamics of a nonautonomous Levins-type metapopulation model subject to colonization pulses. Two intervention strategies are considered: state-dependent impulses, triggered when the fraction of occupied patches reaches a prescribed threshold, and impulses applied at predetermined observation times.

Mathematical model

Between consecutive impulses, the fraction of occupied patches (p(t)) is governed by

[
p'(t)=c(t)p(t)(1-p(t))-e(t)p(t),
]

where (c(t)) and (e(t)) denote the time-dependent colonization and extinction rates, respectively.

Depending on the numerical experiment, colonization pulses are implemented either when the solution reaches a minimum viable threshold (p_{\min}), or at a prescribed sequence of observation times.

Requirements

The simulations were developed in Python and require the following packages:

numpy
pandas
matplotlib
scipy

The main SciPy routines used are:

scipy.integrate.solve_ivp
scipy.integrate.quad
scipy.optimize.minimize_scalar
Numerical procedure

The continuous dynamics between consecutive impulses are numerically integrated using scipy.integrate.solve_ivp. Since no alternative integration method is specified, solve_ivp uses its default RK45 method.

For the state-dependent impulse simulations, the threshold condition is represented by the event function

[
g(t,p)=p-p_{\min}.
]

The event is terminal and only downward crossings are detected (direction = -1). When the threshold is reached, the numerical integration stops at the detected event time. The corresponding colonization pulse is then applied to the state, and the solver is restarted from the same time using the post-impulse state as the new initial condition.

For these simulations, the relative and absolute tolerances are

rtol = 1e-6
atol = 1e-9

For impulses applied at predetermined observation times, the ODE is integrated successively over the intervals between consecutive observation times. At each observation time, the pre-impulse state is obtained from the numerical solution, the corresponding impulse is applied, and the resulting post-impulse state is used as the initial condition for the next integration interval.

Numerical experiments
Figure 2 — State-dependent impulses

The colonization and extinction rates are

[
c(t)=0.5+0.2\sin(t), \qquad
e(t)=0.6+0.1\cos(t),
]

with initial condition (p(0)=0.5) and threshold (p_{\min}=0.15).

The simulations compare the impulse parameters

[
\alpha\in{0.05,0.10,0.15}.
]

Figure 3 — Dependence on the initial condition

The same time-dependent colonization and extinction rates and threshold are considered, with

[
\alpha=0.55
]

and initial conditions

[
p(0)\in{0.65,0.50,0.35,0.20}.
]

Figure 4 — Periodic solutions

For the periodic example,

[
c(t)=0.5+0.1\sin(t), \qquad
e(t)=0.7+0.1\cos(t),
]

and the prescribed period and first threshold-crossing time are

[
T=2\pi,\qquad t_1=1.
]

Three values of (u_{\max}) are considered:

[
u_{\max}\in{12,15,20}.
]

The corresponding threshold (p_{\min}), impulse magnitude (\alpha), and initial condition (p(0)) are computed from the expressions established in Theorem 2 of the manuscript:

(u_{\max})	(p_{\min})	(\alpha)	(p(0))
12	0.08333	0.67618	0.11139
15	0.06667	0.39407	0.08817
20	0.05000	0.22828	0.06544

The code additionally performs a high-accuracy numerical periodicity check. Successive threshold-crossing times (t_k) are compared with

[
t_k=t_1+(k-1)T,
]

and the computed inter-impulse intervals are compared directly with (T=2\pi).

For this verification, tighter numerical tolerances are used:

rtol = 1e-11
atol = 1e-13
Figure 5 — Impulses at fixed observation times

The simulations return to

[
c(t)=0.5+0.2\sin(t), \qquad
e(t)=0.6+0.1\cos(t),
]

with

[
p_{\min}=0.18
]

and observation times

[
t_k=3k.
]

The impulse function is

[
K(p,\alpha)=
\begin{cases}
0, & p<p_{\min},\[2mm]
\displaystyle
\alpha\frac{1-p}{2-(p+p_{\min})},
& p\geq p_{\min}.
\end{cases}
]

The simulations compare (\alpha=0.3,0.5,0.7) for several initial conditions.

Figure 6 — Effect of the observation frequency

The same impulse function is considered with (\alpha=0.3) and (p_{\min}=0.18).

Three periodic observation schedules are compared:

[
t_k=3k,\qquad
t_k=2k,\qquad
t_k=k.
]

This experiment illustrates the effect of the frequency of the observation/intervention times on the metapopulation dynamics.

Output files

Running the script generates the figures in EPS format for direct inclusion in the manuscript.

The code also prints the parameters used in the periodic example and the numerical periodicity verification to the Python console.

Reproducibility

All model parameters, initial conditions, thresholds, impulse magnitudes, observation times, and numerical integration settings required to reproduce the simulations are explicitly specified in the source code.

For the periodic example, the quantities (p_{\min}), (\alpha), and (p(0)) are calculated programmatically from the expressions associated with Theorem 2 rather than being manually introduced.

Citation

If you use this code, please cite the associated manuscript:

Amster, P., Elorreaga, H., Robledo, G., & Sepúlveda, D. (2026). EX-SITU CONSERVATION MEASURES REVISITED: A TIME VARYING METAPOPULATION APPROACH ON THE LEVINS MODEL.

The complete bibliographic information will be added upon publication.

License

Please refer to the repository license for information regarding reuse and distribution of the code.
