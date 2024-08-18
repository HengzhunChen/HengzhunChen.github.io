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

## Code

This part are some independent codes for particular usage.

### FGAFSE [[Github]](https://github.com/HengzhunChen/FGAFSE)

FGAFSE is a MATLAB code that offers an implementation of the Frozen Gaussian Approximation (FGA) solver for the fractional Schrödinger equation (FSE) in the semi-classical regime. In this regime, the solution exhibits high oscillations when the scaled Planck constant $\varepsilon$ is small. The FGA method provides an efficient computational approach by approximating the solution to the Schrödinger equation using an integral representation derived from asymptotic analysis. It offers a highly accurate and efficient method for the evolution of high-frequency wave functions. We also provide numerical examples in both one and two dimensions to demonstrate the accuracy and effectiveness of the FGA method by comparing it with the Time Splitting Spectral Approximation method. 

### Tex2docx [[Github]](https://github.com/HengzhunChen/tex2docx)

This is a guide of using "Pandoc+Panflute" to convert LaTeX file to Docx format. While Pandoc can convert LaTeX to Docx, the conversion is not always be perfect, as the two formats have different capabilities and features. Manual adjustments or fine-tuning may be required after the conversion, to ensure the document appears as intended in the new format. The goal of this project is to showcase the workflow of customized Pandoc, a tailored version of Pandoc that aims to minimize the need for manual adjustments.
