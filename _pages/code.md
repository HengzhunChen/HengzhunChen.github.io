--- 
layout: archive
title: "Code Fields"
permalink: /code/
author_profile: true
---


## Software 

This part are self-contained and mature packages.

### DGESMAT [[Github]](https://github.com/HengzhunChen/DGESMAT)

DGESMAT is a MATLAB toolbox designed for calculating Electronic Structure eigenvalue problems within the Discontinuous Galerkin (DG) framework based on the Kohn-Sham density functional theory (DFT). The flexibility of the DG framework has two significant impacts. Firstly, it allows the utilization of discontinuous basis functions to approximate the continuous Kohn-Sham orbitals, enabling high accuracy in total energy calculations with a small number of basis functions per atom. Secondly, it exhibits high parallel efficiency through a multi-level parallelization scheme, which is beneficial for practical implementations.

DGESMAT aims to simplify the prototyping and testing of new algorithms for solving the Kohn-Sham problem within the DG framework. It achieves this by utilizing MATLAB's parfor to handle part of the parallelization, eliminating the need for complex MPI programming. Additionally, DGESMAT leverages MATLAB's object-oriented features and operator overloading to represent wavefunctions, Hamiltonians, and their operations, making it beginner-friendly for individuals new to this field.

Currently, DGESMAT employs standard pseudopotentials and supports local density approximation (LDA), generalized gradient approximation (GGA), and hybrid functionals. Furthermore, future releases will incorporate time-dependent DFT and post-DFT calculations to expand its capabilities.