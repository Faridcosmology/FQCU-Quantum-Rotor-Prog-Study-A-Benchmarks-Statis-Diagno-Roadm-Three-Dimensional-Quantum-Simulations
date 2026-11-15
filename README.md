# The FQCU Quantum Rotor Programme Study A Benchmarks, Statistical Diagnostics, and a Roadmap to Three-Dimensional Quantum Simulations

Study A — Quantum Rotor Validation: Evidence and Limitations 1
From Harmonic Benchmarks to Nonperturbative
Quantum Rotor Validation
Study A Evidence, Statistical Diagnostics, and a Conditional Roadmap to a
Three-Dimensional Pilot
Md. Faridul Islam Chowdhury
Tanfarid Vision Research Institute, Bogura, Bangladesh
Revised manuscript — November 16, 2026
Abstract
Nonperturbative calculations require independently controlled reference calculations, correctly
normalized estimators, documented sampling effort, and uncertainty estimates tested across
independent replicas. This article consolidates the available Study A evidence for a compact
quantum rotor model considered within the Farid Quantum Command Unit (FQCU) programme.
Finite-step quantum Monte Carlo estimates are compared with deterministic transfer matrices, while
the latter are compared with continuum-time Hamiltonian exact diagonalization. A fixed six-seed
two-rotor campaign yielded ensemble means close to the finite-step references but an anomalously
low energy replica-consistency value at one discretization. Component analysis did not isolate a
single failing kinetic contribution; the replica means exhibited joint charge-sector and
potential-energy co-variation that differed from the pooled short-window covariance estimate.
Component results obtained by deterministic regeneration are distinguished from direct access to
the original archive. A larger Python follow-up is specified and smoke-tested, but its full production
results remain pending. The work is a transparent benchmark and diagnostic record, not a
completed validation of a three-dimensional sampler or a determination of a quantum critical
coupling.
1. Introduction
The transition from a harmonic calculation to an interacting quantum simulation is methodological as
well as computational. Harmonic results supply spectra and limiting fluctuation measures. An
interacting calculation must also establish that its algorithm samples the intended distribution, its
observables are normalized consistently, and its error analysis accounts for correlated
measurements.
Study A addresses these requirements through two interacting compact rotors. It is not an attempt
to infer a bulk transition from two sites. It is a reference calculation and diagnostic record intended
to precede larger-system work. The validation sequence is: define the model, construct independent
references, assess sampling and estimators, and only then undertake larger-lattice calculations.
The FQCU interpretation motivates the research programme but does not alter the rotor weights
used here. The benchmark results are properties of the stated model and are not unique evidence for
that interpretation. Completed calculations and pending studies are separated throughout.
2. Hamiltonian, units, and compact quantization
Let I and J be positive. Angular momentum has integer spectrum in the conventional periodic rotor
representation. Unitary phase operators specify compact quantization without requiring an
unrestricted global angle commutator:
(1)
Study A — Quantum Rotor Validation: Evidence and Limitations 2
(2)
The interaction is defined by the Hermitian combination of unitary phase operators; cosine notation
is used as shorthand:
(3)
(4)
The kinetic coefficient is gQ squared, not gQ. The shifted interaction gives zero classical potential
energy for alignment. Absolute energies from an unshifted minus-cosine Hamiltonian must be
translated before comparison.
(5)
(6)
These definitions are used in all thermal factors, transfer weights, and estimators. In decoupled tests
the spatial interaction is disabled while retaining a positive reference energy unit; one must not
literally set that unit J to zero while keeping these dimensionless parameters finite.
3. Two independent reference levels
At fixed time step, QMC is compared with a deterministic transfer calculation for the same
discretized model. This tests sampling and observable implementation without contamination by the
difference between finite and zero time step. The transfer calculation is then compared with
Hamiltonian exact diagonalization (ED), isolating imaginary-time discretization from stochastic
noise.
Neither comparison is automatically exact. The transfer calculation requires momentum-basis and
floating-point checks. ED requires a converged local basis and, for thermal quantities, a controlled
thermal trace. The references are computationally independent formulations of the benchmark, not
independent experimental observations.
4. Deterministic two-rotor benchmarks
The reference geometry is a periodic two-site multigraph with two spatial links joining the same pair.
The Hamiltonian is
(7)
At gQ = 1.2 and b = 2, the converged Hamiltonian calculation gives
(8)
The full truncated spectrum was evaluated in conserved-total-angular-momentum blocks. A dense
calculation at local cutoff 4 agreed with the block calculation within approximately 7 × 10⁻¹⁶ in the
compared observables. Increasing the local cutoff from 10 to 14 changed the energy by about 1.1 ×
10⁻¹⁶ and the bond cosine by zero at printed floating-point precision. Such agreement is a numerical
convergence check, not a certificate for every displayed digit.
Study A — Quantum Rotor Validation: Evidence and Limitations 3
4.1. Finite-step transfer operator
Use the symmetric transfer product with dimensionless kinetic and potential operators. In a fixed-M
block, let m denote the angular momentum of the first rotor and define qM(m) = m² + (M−m)². The
matrix used in the deterministic reference is
(9)
(10)
In these formulas m-prime is a second basis index. A finite momentum basis is used numerically.
Eigenvalues smaller than 10⁻¹⁴ of each block maximum were omitted in the reference trace
calculation; for the reported powers L_tau ≥ 10 this contribution is strongly suppressed. Cutoff
comparisons are retained in the numerical record. Differentiation of the finite-step partition function
gives the thermodynamic energy estimator:
(11)
Table 1. Deterministic finite-step references at gQ = 1.2 and b = 2. Values are rounded numerical outputs, not
rigorous intervals.
a Lτ e/J Cb
0.20 10 0.7350070441 0.6730785682
0.10 20 0.7404684291 0.6698685806
0.05 40 0.7418608335 0.6690508366
0.025 80 0.7422106842 0.6688454176
0.0125 160 0.7422982572 0.6687940009
The energy discrepancy decreases approximately fourfold upon halving a, consistent with a leading
a² correction. This observed pattern supports the expected discretization behaviour; it is not a
rigorous all-orders extrapolation bound. A QMC run cannot resolve this trend reliably when its
sampling errors exceed the differences between time steps.
5. Integer-current sampling and observables
The discrete-time current representation has positive link weights
(12)
The constant potential offset is retained when deriving absolute energies. Closed configurations
satisfy discrete divergence freedom. The bond estimator uses the logarithmic Bessel derivative [4]:
(13)
The prime in this equation denotes differentiation with respect to the argument. Let Cb be the
ensemble mean of this link estimator, averaged over spatial links. For an individual measured
configuration, use the corresponding instantaneous estimator. The per-site dimensionless energy
estimators are
(14)
Study A — Quantum Rotor Validation: Evidence and Limitations 4
The temporal overbar averages all temporal links. These are thermodynamic estimators for the
discretized partition function; their continuum-time interpretation requires the time-step limit.
Individual bond-estimator values need not lie between minus one and one and must not be clipped.
A closed worm call includes possible rejection self-transitions and has variable length. One worm is
not a volume-normalized sweep. Measurements are taken only after closure and explicit conservation
checks. A timeout aborts the chain rather than leaving an open state eligible for measurement or
reuse.
6. The completed six-seed campaign
The recorded campaign used gQ = 1.2, b = 2, time steps 0.20 and 0.10, and seeds 101, 211, 307,
401, 503, and 601. Each replica had 1,000 warm-up intervals and 6,000 production intervals, with 2
L_tau worms per interval. The compiled implementation used std::mt19937_64. It is not a bitwise
execution of a NumPy-PCG64 implementation.
Across the two steps, the campaign completed 2,520,000 worms, 62,773,834 attempted link moves,
and 72,000 production measurements. Every measured configuration passed the implemented
divergence and winding-integrality checks. No timeout occurred. These are invariant checks, not
proof of equilibration or correct statistical coverage.
Table 2. Six-seed ensemble results. Errors are primary-observable Gamma-method estimates at S_tau = 1.5;
pulls use the finite-step references.
a Observable Estimate ± SE Pull Replica Q
0.20 E/J 0.734942 ± 0.003370 −0.02 0.00149
0.20 Cb 0.673526 ± 0.003023 +0.15 0.0831
0.10 E/J 0.742485 ± 0.003277 +0.62 0.704
0.10 Cb 0.667632 ± 0.002918 −0.77 0.858
The mean estimates are close to the references, but the a = 0.20 energy replica-consistency value is
anomalously low. A small grand-mean pull cannot replace consistency of the independent replicas.
All seeds are retained. No seed was removed or rerun to improve its Q-value.
6.1. Error analysis and computational work
The analysis follows the primary-observable Gamma-method framework [1], using an independent
implementation rather than claiming a complete or bitwise-verified UWerr port. For an observable F,
the pooled autocovariance integrates correlations within replicas; it does not concatenate separate
replicas across their boundaries.
(15)
The implemented finite-mean correction adds C_raw/N to the autocovariance sequence and uses that
corrected sequence consistently. Automatic-window settings S_tau = 1.5, 2, and 3 were inspected.
Errors and replica Q-values remain finite-sample estimates, not exact guarantees. Weak slow
contributions may be missed by a short selected window [1,3].
Mean attempted worm lengths were approximately 14.99 and 29.86 at L_tau = 10 and 20,
respectively. Thus a fixed number of worms proportional to space-time volume did not equalize
link-update work. Autocorrelation times must be interpreted with their measurement schedule and
work telemetry.
Study A — Quantum Rotor Validation: Evidence and Limitations 5
7. Component decomposition of the replica anomaly
(16)
(17)
Total charge is obtained from one time slice after checking equality across slices. Summing all slices
returns L_tau times M and would inflate KM by L_tau squared. The component table below uses the
corrected normalization.
Table 3. Component results at a = 0.20 and L_tau = 10. Each component uses its own automatic window at S_tau
= 1.5. Autocorrelation times are in saved measurement intervals.
Component Mean SE τint Replica Q
E 0.734942 0.003370 0.807 0.00149
K 0.408468 0.002340 0.954 0.245
V 0.326474 0.003023 0.732 0.0831
KM 0.149515 0.001556 0.956 0.123
Krel 0.258953 0.001677 0.883 0.291
The kinetic and charge-sector marginals do not isolate an obvious kinetic failure. At a = 0.10, the
corresponding component Q-values are approximately 0.704 for E, 0.475 for K, 0.858 for V, 0.472 for
KM, and 0.695 for Krel.
7.1. Common-window covariance
(18)
For the joint comparison, every component uses the common energy window W = 7 at a = 0.20. The
between-replica covariance is the sample covariance of the six equally weighted replica means
divided by six. It is a noisy six-replica cross-check of the covariance of the estimated grand mean.
Table 4. Covariances of estimated means at a = 0.20 and common W = 7. All entries are multiplied by 10⁶.
Quantity Pooled Γ estimate Between-replica
Var(mean K) 5.504 7.321
Var(mean V) 9.133 17.790
Cov(mean K, mean V) −1.642 +9.693
Var(mean E) 11.354 44.497
The pooled estimate includes negative kinetic-potential covariance and partial cancellation. The six
replica means instead show positive co-variation, increasing the observed energy scatter. The total
variance estimates differ by a factor of about 3.92. Within the finer decomposition, Cov(mean KM,
mean V) is approximately −0.0891 × 10⁻⁶ in the pooled analysis and +8.0255 × 10⁻⁶ across replicas.
This is an exploratory description, not an identified dynamical mechanism.
Study A — Quantum Rotor Validation: Evidence and Limitations 6
7.2. Provenance of the component analysis
The component analysis was performed by deterministic regeneration because the original archive
was unavailable at that stage. The recorded generator, seeds, transition rules, and schedule were
reconstructed. Energy means and Q-values matched their earlier six-decimal reported values, and
the attempted-move total matched exactly. No bytewise comparison with the original archive was
performed. Regenerated histories must remain labelled as such.
8. Physical interpretation: no centrifugal barrier
(19)
At fixed M, the first term is constant and creates no centrifugal potential in the relative angle.
However, integer individual momenta require r and M to have the same parity. Collective and
relative motion are therefore not an unrestricted independent tensor product. Compact-coordinate
and parity constraints must be retained in interpreting a separated rotor description [2].
(20)
A fixed-charge continuum-time Hamiltonian calculation at gQ = 1.2 and b = 2 gives conditional
potential energy per site about 0.336371 for M = 0 and 2, and 0.325614 for M = 1 and 3. These are
conditional continuum-time values, not the unconditional finite-a targets in Table 1. Their parity
dependence rules out the proposed monotonic centrifugal-barrier interpretation. The observed
replica co-variation alone is not a new physical mechanism.
9. Corrected follow-up: specified but not completed
The corrected Python follow-up has undergone syntax, execution, invariant, and deterministic
charge-normalization smoke tests. Its full six-seed production results remain pending. No results
from that pending run are supplied here.
Table 5. Specified defaults for the pending Python robustness test. This is a draft protocol, not a claim of external
preregistration.
Parameter Specification
Geometry Two-site periodic doubled-link graph
gQ; b 1.2; 2
a; Lτ 0.20; 10
Seeds 101, 211, 307, 401, 503, 601
Generator NumPy PCG64
Worms per interval 80
Warm-up; production 2,000; 12,000 intervals per seed
Total worms 6,720,000
The follow-up changes measurement spacing, warm-up, production work, and implementation
together. It is a robustness test rather than a unique isolation of one factor. Matching seed numbers
across PCG64 and mt19937_64 does not yield matching trajectories. A completed distributional
cross-implementation check remains a task, not an accomplished result.
Improved agreement would strengthen the evidence; persistent discrepancies would motivate
additional diagnosis. Neither outcome conclusively validates all observables or uniquely
Study A — Quantum Rotor Validation: Evidence and Limitations 7
demonstrates an ergodicity bottleneck. Original anomalous results must be retained alongside later
results. No seed should be excluded solely for producing an inconvenient diagnostic.
Slow-mode-informed analyses require justified information about the decay scale [3]. An assumed
exponential continuation is a conditional sensitivity calculation unless its amplitude and slow time
scale support a defensible conservative interpretation. It must not be tuned merely to obtain a
preferred Q-value.
10. Conditions for a three-dimensional pilot
A periodic 2³ cubic pilot, with eight sites and 24 positive-direction spatial links under the
doubled-link convention, is the next proposed stage. Its initial goals are invariant checks, sampling
behaviour, charge-sector exploration, observable-specific autocorrelation, time-step sensitivity, and
cost per effectively independent estimate. It is not a critical-point determination.
Couplings should be labelled lower, intermediate, and higher test values rather than assigned
ordered or disordered phases in advance. At b = 2 the pilot remains a finite-temperature study
unless temperature convergence is separately tested. Two time steps supply sensitivity information,
not a controlled continuum extrapolation. A dated deposit and verified identifier are required before
describing the protocol as externally preregistered.
11. Conclusion
Study A provides an explicit benchmark architecture. Finite-step deterministic references separate
sampling questions from time discretization; Hamiltonian ED provides a separately converged
continuum-time target. The completed six-seed campaign agrees with those references in its
ensemble means but exhibits an unresolved replica anomaly at one step.
Component analysis reveals joint fluctuations across replica means rather than an isolated,
demonstrated kinetic-sector failure. The corrected follow-up is ready for full execution, but its
production results are not yet part of the evidence record. Future tables and covariance matrices
must be reported alongside the original anomaly, with their provenance and uncertainty
assumptions.
The contribution is a more precise computational evidence record and a better-defined next test. It is
not a closed sampler-validation study, a three-dimensional critical-coupling result, an experimentally
confirmed microscopic model, or a unique signature of FQCU.
12. Data and code availability
The evidence record refers to the previously generated campaign, diagnostic, and follow-up
packages. This manuscript revision does not itself re-execute the production campaigns. Before
public archival submission, the author should preserve the exact source versions, original and
regenerated histories separately, compiler and library metadata, analysis settings, and execution
logs. No repository DOI or registered study identifier is asserted here.
References
[1] U. Wolff, “Monte Carlo errors with less errors,” Computer Physics Communications 156, 143–153 (2004);
erratum 176, 383 (2007). arXiv:hep-lat/0306017v4.
[2] Z. Liu, J. Leong, S. Nimmrichter, and V. Scarani, “Quantum gears from planar rotors,” Physical Review E 99,
042202 (2019). arXiv:1810.13121.
[3] S. Schaefer, R. Sommer, and F. Virotta, “Critical slowing down and error analysis in lattice QCD
simulations,” Nuclear Physics B 845, 93–119 (2011). arXiv:1009.5228.
[4] NIST Digital Library of Mathematical Functions, Section 10.29, “Recurrence Relations and Derivatives” for
modified Bessel functions.
