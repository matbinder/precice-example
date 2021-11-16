# Partitioned heat conduction
Source: https://github.com/precice/tutorials/tree/master/partitioned-heat-conduction

Test it on: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/matbinder/precice-example/HEAD?labpath=precice.ipynb)

## Summary
We solve a simple heat equation. The domain is partitioned and the coupling is established in a Dirichlet-Neumann fashion.

## Setup

We solve a partitioned heat equation. For information on the non-partitioned case, please refer to [1, p.37ff]. In this tutorial the computational domain is partitioned and coupled via preCICE. The coupling roughly follows the approach described in [2].

![Case setup of partitioned-heat-conduction case](images/tutorials-partitioned-heat-conduction-setup.png)

Case setup from [3]. `D` denotes the Dirichlet participant and `N` denotes the Neumann participant.

The heat equation is solved on a rectangular domain `Omega = [0,2] x [0,1]` with given Dirichlet boundary conditions. We split the domain at `x_c = 1` using a straight vertical line, the coupling interface. The left part of the domain will be referred to as the Dirichlet partition and the right part as the Neumann partition. To couple the two participants we use Dirichlet-Neumann coupling. Here, the Dirichlet participant receives Dirichlet boundary conditions (`Temperature`) at the coupling interface and solves the heat equation using these boundary conditions on the left part of the domain. Then the Dirichlet participant computes the resulting heat flux (`Flux`) from the solution and sends it to the Neumann participant. The Neumann participant uses the flux as a Neumann boundary condition to solve the heat equation on the right part of the domain. We then extract the temperature from the solution and send it back to the Dirichlet participant. This establishes the coupling between the two participants.

This simple case allows us to compare the solution for the partitioned case to a known analytical solution (method of manufactures solutions, see [1, p.37ff]). For more usage examples and details, please refer to [3, sect. 4.1].

## Visualization

Output is written into the folders `fenics/out`.

For FEniCS you can visualize the content with paraview by opening the `*.pvd` files. The files `Dirichlet.pvd` and `Neumann.pvd` correspond to the numerical solution of the Dirichlet, respectively Neumann, problem, while the files with the prefix `ref` correspond to the analytical reference solution, the files with `error` show the error and the files with `ranks` the ranks of the solvers (if executed in parallel).

![Animation of the partitioned heat equation](images/tutorials-partitioned-heat-conduction-FEniCS-movie.gif)

Visualization in paraview for `x_c = 1.5`.

## References

[1] Hans Petter Langtangen and Anders Logg. "Solving PDEs in Minutes-The FEniCS Tutorial Volume I." (2016). [pdf](https://fenicsproject.org/pub/tutorial/pdf/fenics-tutorial-vol1.pdf)  
[2] Azahar Monge and Philipp Birken. "Convergence Analysis of the Dirichlet-Neumann Iteration for Finite Element Discretizations." (2016). Proceedings in Applied Mathematics and Mechanics. [doi](https://doi.org/10.1002/pamm.201610355)  
[3] Benjamin Rüth, Benjamin Uekermann, Miriam Mehl, Philipp Birken, Azahar Monge, and Hans Joachim Bungartz. "Quasi-Newton waveform iteration for partitioned surface-coupled multiphysics applications." (2020). International Journal for Numerical Methods in Engineering. [doi](https://doi.org/10.1002/nme.6443)  
