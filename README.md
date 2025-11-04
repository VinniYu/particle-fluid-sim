# Predictive-Corrective Incompressible SPH

**Breakdown**

Authors: Barbara Solenthaler and Renato Pajarola from University of Zurich
Main Idea: Enforcing incompressibility in particle-based fluid simulations is computationally expensive
Prior solutions:
1. Weakly Compressible SPH (WCSPH): Uses a stiff equation of state (EOS) to model pressure, but requires small time steps due to the CFL condition, making it computationally expensive
2. Incompressible SPH (ISPH): Solves a pressure Poisson equation to project velocities onto a divergence-free space, but has complex formulation on unstructured particle configurations

PCI-SPH combines advantages of both methods:
1. Don't use EOS or solve Poisson Equation
2. Predicts future particle positions and densities
3. Actively propagates density fluctuation information through the fluid
4. Updates pressure values until the target density is satisfied

```cpp title=Algorithm_Flow
initialize pressure to 0
while density exceeds threshold
	predict velocities and positions
	predict densities and density variations
	update pressure based on density error
	compute pressure forces
compute final velocities and positions
```

Pressure derivation
1. The paper presents a detailed mathematical derivation relating density fluctuations to pressure
2. Uses approximations to keep the pressure update rule simple/efficient
3. Pressures accumulate over iterations

Implementation Details:
1. Neighborhood approximation for efficiency
2. Minimum number of iterations (3) to reduce temporal pressure fluctuations

**RESULTS**
Performance metrics
1. PCISPH outperforms WCSPH by more than an order of magnitude
2. For 1% density fluctuation threshold: 15-16x speedup
3. For 0.1% density fluctuation threshold: 55x speedup
Convergence Analysis
4. Average of 3-4 convergence iterations per physics step
5. Density error approximately halves after first iteration
6. Robust with no divergence problems
Visual Results
7. Visual comparison shows PCISIP computations in full agreement with WCSPH
8. Higher res examples
9. Realistic wave breaking and splashing behavior

**SIGNIFICANCE**
1. significantly larger time steps than WCSPH
2. lower computational cost per physics update than ISPH
3. high-res fluid animations with realistic incompressibility
4. less parameter tuning
