# Seed literature

Starting list of literature. The agent may expand it, but should start with these articles.

## SOAP and local environment descriptors
- Bartók, Kondor, Csányi (2013). "On representing chemical environments".
  What it's about: definition of SOAP, power spectrum, comparison with other descriptors.
  Why: basic formula for encoding the environment of a Li atom.
  PDF: data/papers/bartok2013_soap.pdf

- Behler (2011). "Atom-centered symmetry functions for constructing high-dimensional neural network potentials".
  What it's about: symmetry functions, invariance to rotations and permutations.
  Why: understand which descriptor properties are important for GPR.
  PDF: data/papers/behler2011_acsf.pdf

## Set representation
- Xie et al. (2025). "Advances in Set Function Learning: A Survey of Techniques and Applications".
  What it's about: how to represent a set of elements as a fixed-length vector.
  Why: choosing a pooling variant for different numbers of intercalated atoms in the matrix.
  PDF: data/papers/xie2025_representation.pdf

- Corso et al. (2020). "Principal Neighbourhood Aggregation for Graph Nets".
  What it's about: application of statistical pooling.
  Why: a concrete example of representing a set as a fixed-length vector — statistical pooling.
  PDF: data/papers/corso2020_stats.pdf

## GPR and BO
- Mattias Seeger (2004). "Gaussian Processes for Machine Learning".
  What it's about: GPR theory, kernels, marginal likelihood.
  Why: basis for kernel selection and training mode.
  PDF: data/papers/seeger2004_gpr_basics.pdf

- Shahriari et al. (2016). "Taking the human out of the loop: A review of Bayesian optimization".
  What it's about: BO overview, acquisition functions, applications.
  Why: understand how to embed GPR into the BO loop.
  PDF: data/papers/shahriari2016_bo_basics.pdf

## Kernel selection
- Duvenaud et al. (2013). "Structure discovery in nonparametric regression through compositional kernel search".
  What it's about: searching for kernel structure as a composition of base kernels.
  Why: subproblem of choosing a kernel combination.
  PDF: data/papers/duvenaud13_composition_kernel.pdf

- Shuaib et al. (2025). "Multikernel similarity-based clustering of amorphous systems and machine-learned interatomic potentials by active learning".
  What it's about: a hybrid similarity kernel that demonstrates the integration of short- and long-range descriptors using the averaged kernel method.
  Why: application of a similarity kernel to chemical systems; the article is similar to my idea for batteries, but implemented on other systems.
  PDF: data/papers/shuaib2024_multikernel_amorphous.pdf

- Lei et al. (2016). "Localized Multiple Kernel Learning—A Convex Approach".
  What it's about: a convex approach to multiple kernel learning.
  Why: description of an approach to solving the kernel parameter selection problem as a convex problem. Useful for the second project on this topic, but for the course "Convex Analysis and Optimization".
  PDF: data/papers/lei2016_multiple_kernel.pdf
  