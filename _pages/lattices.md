---
title: "Lattices"
permalink: /lattices/
---

Lattice-based cryptography is the leading approach to post-quantum cryptography. This page gives a short introduction to the area and points to a small selection of resources that I recommend to students and colleagues who want to learn it.

## What is a lattice?

A lattice is a regular grid of points in *n*-dimensional space. Given linearly independent basis vectors **b**<sub>1</sub>, …, **b**<sub>n</sub>, the lattice consists of all their integer linear combinations *z*<sub>1</sub>**b**<sub>1</sub> + … + *z*<sub>n</sub>**b**<sub>n</sub>. The same lattice has infinitely many bases: a "good" basis of short, nearly orthogonal vectors makes many problems easy, while a "bad" basis of long, skewed vectors hides the structure of the lattice. Finding a short non-zero vector (the Shortest Vector Problem, SVP) or the lattice point closest to a given target (the Closest Vector Problem, CVP) is believed to be hard in high dimensions, for classical and quantum computers alike. The best known algorithms, lattice reduction such as BKZ combined with sieving or enumeration, take time exponential in the dimension.

## SIS and LWE

Almost all of modern lattice cryptography rests on two average-case problems, both defined over the integers modulo a number *q*. What makes them special is that they come with *worst-case to average-case reductions*: breaking a random instance is provably at least as hard as solving certain lattice problems in the worst case, which gives lattice cryptography unusually strong theoretical foundations.

The **Short Integer Solution (SIS)** problem asks, given a uniformly random matrix **A** ∈ ℤ<sub>q</sub><sup>n×m</sup> with more columns than rows, to find a non-zero integer vector **x** with small norm such that **Ax** = **0** mod *q*. Without the norm bound this is easy linear algebra, but requiring **x** to be short turns it into the problem of finding a short vector in the lattice of all integer solutions. If SIS is hard, then the function **x** ↦ **Ax** mod *q* restricted to short inputs is collision-resistant, since any collision **x** ≠ **x**′ gives a short solution **x** − **x**′. This directly gives hash functions and commitment schemes, and, combined with trapdoors or the Fiat–Shamir with aborts technique, digital signatures. Ajtai (1996) showed that solving SIS on average is as hard as approximating short vector problems in *every* lattice of a related dimension.

The **Learning with Errors (LWE)** problem asks to recover a secret vector **s**, given a random matrix **A** and **b** = **As** + **e** mod *q*, where **e** is a small random error vector. In the decision version, the task is only to distinguish (**A**, **b**) from uniformly random. Without the error, **s** could be found by Gaussian elimination; the small noise is what makes the problem hard. LWE directly gives public-key encryption: in Regev's scheme, the public key is (**A**, **b**), a bit is encrypted by adding a random subset of the samples and hiding the bit in the most significant part of the result, and the secret key **s** removes everything except a small amount of noise. Regev (2005) proved LWE as hard as worst-case lattice problems under a quantum reduction, later complemented by classical reductions for suitable parameters. LWE is the basis for most modern lattice-based encryption, including fully homomorphic encryption.

The two problems are closely related: a short vector **x** with **x**<sup>T</sup>**A** = **0** mod *q*, that is, a SIS solution for the transposed matrix, gives **x**<sup>T</sup>**b** = **x**<sup>T</sup>**e**, which is small and hence distinguishes LWE samples from random. The best known attacks on both problems therefore come from lattice reduction. For efficiency, practical schemes use structured variants in which **A** consists of blocks of polynomials in a ring such as ℤ<sub>q</sub>[X]/(X<sup>n</sup> + 1). Ring-SIS/LWE and Module-SIS/LWE reduce key sizes from quadratic to linear in the dimension and allow fast multiplication with the number-theoretic transform, while retaining worst-case hardness guarantees for structured (ideal and module) lattices.

## Why lattices for cryptography?

No quantum algorithm is known to solve these problems significantly faster than classical ones, in contrast to RSA and elliptic-curve cryptography, which are broken by Shor's algorithm. In August 2024, NIST published its first post-quantum standards: [ML-KEM](https://csrc.nist.gov/pubs/fips/203/final) (FIPS 203, based on Kyber) for key encapsulation and [ML-DSA](https://csrc.nist.gov/pubs/fips/204/final) (FIPS 204, based on Dilithium) for digital signatures, both built on Module-LWE and Module-SIS. The [CRYSTALS website](https://pq-crystals.org) is the home of the Kyber and Dilithium projects, and the [NIST Post-Quantum Cryptography project](https://csrc.nist.gov/projects/post-quantum-cryptography) tracks the remaining standardization work.

Beyond encryption and signatures, lattices are remarkably versatile. They underlie all practical fully homomorphic encryption schemes, and support advanced primitives such as zero-knowledge proofs, threshold and blind signatures, and verifiable encryption and mix-nets, which is where much of my own [research](/research/) is focused.

## NTRU

NTRU, introduced by Hoffstein, Pipher, and Silverman in 1998, was one of the first practical lattice-based public-key cryptosystems. It works in a polynomial ring, where the public key is a quotient *h* = *g*/*f* mod *q* of two secret polynomials *f* and *g* with small coefficients. Recovering such a short pair from *h* amounts to finding a short vector in a highly structured "NTRU lattice". This structure gives very compact keys and ciphertexts, and NTRU lattices admit efficient trapdoors: the signature scheme [Falcon](https://falcon-sign.info), which uses hash-and-sign with fast Fourier sampling over NTRU lattices, is being standardized by NIST as FN-DSA (FIPS 206). NTRU-based key exchange is already deployed in practice, for example through [Streamlined NTRU Prime](https://ntruprime.cr.yp.to), which is part of the default hybrid key exchange in OpenSSH. Parameters must be chosen with care, since NTRU with a very large modulus relative to the secret ("overstretched" NTRU) is vulnerable to dedicated lattice attacks. In our own work, we have used NTRU to build [more efficient lattice-based electronic voting](https://eprint.iacr.org/2023/933) and [distributed key generation for NTRU](https://eprint.iacr.org/2026/1854).

## A brief history

The table below gives a rough and admittedly biased history of the foundations of modern lattice-based cryptography, shaped by my own research interests; many other important works have pushed the frontier of the field.

| Year | Milestone | Paper |
| --- | --- | --- |
| 1982 | Lattice basis reduction (LLL), the starting point of lattice cryptanalysis | Lenstra, Lenstra, and Lovász: [Factoring Polynomials with Rational Coefficients](https://doi.org/10.1007/BF01457454) |
| 1996 | Worst-case to average-case hardness for lattice problems (SIS) | Ajtai: [Generating Hard Instances of Lattice Problems](https://eccc.weizmann.ac.il/report/1996/007/) |
| 1998 | NTRU, one of the first practical lattice-based cryptosystems | Hoffstein, Pipher, and Silverman: [NTRU: A Ring-Based Public Key Cryptosystem](https://doi.org/10.1007/BFb0054868) |
| 2005 | Learning with Errors and Regev encryption | Regev: [On Lattices, Learning with Errors, Random Linear Codes, and Cryptography](https://cims.nyu.edu/~regev/papers/qcrypto.pdf) |
| 2008 | Lattice trapdoors and hash-and-sign signatures | Gentry, Peikert, and Vaikuntanathan: [Trapdoors for Hard Lattices and New Cryptographic Constructions](https://eprint.iacr.org/2007/432) |
| 2009 | The first fully homomorphic encryption scheme | Gentry: [Fully Homomorphic Encryption Using Ideal Lattices](https://doi.org/10.1145/1536414.1536440) |
| 2009 | Fiat–Shamir with aborts, the technique behind efficient lattice signatures without trapdoors | Lyubashevsky: [Fiat-Shamir with Aborts: Applications to Lattice and Factoring-Based Signatures](https://www.iacr.org/archive/asiacrypt2009/59120596/59120596.pdf) |
| 2010 | Ring-LWE | Lyubashevsky, Peikert, and Regev: [On Ideal Lattices and Learning with Errors over Rings](https://eprint.iacr.org/2012/230) |
| 2012 | Lattice signatures from SIS and LWE via Fiat–Shamir with aborts, the basis of Dilithium/ML-DSA | Lyubashevsky: [Lattice Signatures Without Trapdoors](https://eprint.iacr.org/2011/537) |
| 2012 | Simpler and more efficient trapdoors | Micciancio and Peikert: [Trapdoors for Lattices: Simpler, Tighter, Faster, Smaller](https://eprint.iacr.org/2011/501) |
| 2012–2013 | Practical FHE: BGV, BFV, and GSW | Brakerski, Gentry, and Vaikuntanathan: [FHE without Bootstrapping](https://eprint.iacr.org/2011/277); Brakerski: [FHE without Modulus Switching](https://eprint.iacr.org/2012/078); Fan and Vercauteren: [Somewhat Practical FHE](https://eprint.iacr.org/2012/144); Gentry, Sahai, and Waters: [Homomorphic Encryption from LWE](https://eprint.iacr.org/2013/340) |
| 2015 | Module-SIS and Module-LWE, the basis of ML-KEM and ML-DSA | Langlois and Stehlé: [Worst-Case to Average-Case Reductions for Module Lattices](https://eprint.iacr.org/2012/090) |
| 2017 | Falcon, hash-and-sign signatures over NTRU lattices, the basis of FN-DSA | Fouque, Hoffstein, Kirchner, Lyubashevsky, Pornin, Prest, Ricosset, Seiler, Whyte, and Zhang: [Falcon: Fast-Fourier Lattice-Based Compact Signatures over NTRU](https://falcon-sign.info/falcon.pdf) |
| 2018 | Kyber, the basis of ML-KEM | Bos, Ducas, Kiltz, Lepoint, Lyubashevsky, Schanck, Schwabe, and Stehlé: [CRYSTALS-Kyber: A CCA-Secure Module-Lattice-Based KEM](https://eprint.iacr.org/2017/634) |
| 2018 | Dilithium, the basis of ML-DSA | Ducas, Lepoint, Lyubashevsky, Schwabe, Seiler, and Stehlé: [CRYSTALS-Dilithium: Digital Signatures from Module Lattices](https://eprint.iacr.org/2017/633) |
| 2018 | Efficient commitments from Module-SIS/LWE (BDLOP), a building block for lattice-based zero-knowledge proofs | Baum, Damgård, Lyubashevsky, Oechsner, and Peikert: [More Efficient Commitments from Structured Lattice Assumptions](https://eprint.iacr.org/2016/997) |
| 2022 | Short and general lattice-based zero-knowledge proofs (LNP) | Lyubashevsky, Nguyen, and Plançon: [Lattice-Based Zero-Knowledge Proofs and Applications: Shorter, Simpler, and More General](https://eprint.iacr.org/2022/284) |
| 2023 | Compact succinct proofs from Module-SIS (LaBRADOR) | Beullens and Seiler: [LaBRADOR: Compact Proofs for R1CS from Module-SIS](https://eprint.iacr.org/2022/1341) |

## Where to start

If you want to understand how ML-KEM and ML-DSA work, start with Vadim Lyubashevsky's [Basic Lattice Cryptography: The concepts behind Kyber (ML-KEM) and Dilithium (ML-DSA)](https://eprint.iacr.org/2024/1287) (2024). It is a self-contained tutorial by one of the designers of both schemes, explaining the underlying mathematical concepts and design decisions, as well as the main ideas behind other lattice-based KEMs such as Frodo and NTRU.

For a broader and more theoretical view, Chris Peikert's survey [A Decade of Lattice Cryptography](https://eprint.iacr.org/2015/939) covers the foundations of SIS and LWE, worst-case hardness, ring-based variants, trapdoors, and advanced constructions such as fully homomorphic encryption. Oded Regev's short survey [The Learning with Errors Problem](https://cims.nyu.edu/~regev/papers/lwesurvey.pdf) is a good companion for understanding LWE itself.

## Courses and lecture notes

Chris Peikert's graduate course [Lattices in Cryptography](https://github.com/cpeikert/LatticesInCryptography) (University of Michigan) has the most up-to-date and comprehensive lecture notes on lattice cryptography. The notes are maintained on GitHub, were used most recently in 2026, and cover everything from the shortest vector problem and the LLL algorithm to SIS, LWE, and digital signatures.

As a complement with more emphasis on algorithms, complexity, and cryptanalysis, Daniele Micciancio's course [Lattice Algorithms and Applications](https://cseweb.ucsd.edu/classes/fa21/cse206A-a/) (UC San Diego) has excellent lecture notes on the geometry of lattices, the LLL algorithm, duality, harmonic analysis, and the hardness of lattice problems.

## Talks

Vinod Vaikuntanathan's colloquium talk [Lattices and Cryptography: A Match Made in Heaven](https://youtu.be/5LGwaICJ5sw) is an accessible one-hour overview of why lattices have become central to cryptography. For an in-depth series of lectures by leading researchers in the field, the [Lattices: Algorithms, Complexity, and Cryptography Boot Camp](https://simons.berkeley.edu/workshops/lattices-algorithms-complexity-cryptography-boot-camp) at the Simons Institute (2020) is still the best starting point, and the workshops of the [full Simons program](https://simons.berkeley.edu/programs/lattices2020) cover more advanced topics.

## Homomorphic encryption

All practical fully homomorphic encryption (FHE) schemes, such as BGV, BFV, CKKS, and TFHE, are based on (Ring-)LWE. The [Survey on Fully Homomorphic Encryption, Theory, and Applications](https://eprint.iacr.org/2022/1602) by Marcolla et al. (Proceedings of the IEEE, 2022) gives a good overview of the schemes and their applications. The community site [FHE.org](https://fhe.org/resources/) maintains an up-to-date collection of tutorials, courses, conference talks, and libraries, and [OpenFHE](https://openfhe.org) is a widely used open-source library implementing all the major schemes.

## Zero-knowledge proofs

Lattice-based zero-knowledge proofs have become efficient enough for real applications such as anonymous credentials and blind signatures. [Lattice-Based Zero-Knowledge Proofs and Applications: Shorter, Simpler, and More General](https://eprint.iacr.org/2022/284) by Lyubashevsky, Nguyen, and Plançon (CRYPTO 2022) presents the modern framework for proving linear relations and norm bounds, and [LaBRADOR](https://eprint.iacr.org/2022/1341) by Beullens and Seiler (CRYPTO 2023) gives compact succinct proofs from Module-SIS. The best way to get started in practice is the [LaZer library](https://github.com/lazer-crypto/lazer) ([paper](https://eprint.iacr.org/2024/1846), ACM CCS 2024), which lets you specify lattice relations and norm bounds in Python and automatically generates the corresponding proof system, with demos for blind signatures, anonymous credentials, and proofs of Kyber keys.

## Advanced signatures

Threshold signatures distribute the signing key among several parties so that no single party can sign alone. [Threshold Raccoon](https://eprint.iacr.org/2024/184) by del Pino et al. (Eurocrypt 2024) was the first efficient lattice-based threshold signature from standard assumptions. More recent schemes include our [Olingo](https://eprint.iacr.org/2025/1789), which adds distributed key generation and identifiable abort (ACM CCS 2026). NIST is currently standardizing threshold schemes through its [Multi-Party Threshold Cryptography project](https://csrc.nist.gov/projects/threshold-cryptography), and the [submissions to the first call](https://csrc.nist.gov/Projects/threshold-cryptography/tcall-1) include several lattice-based threshold signatures.

## Cryptanalysis and tools

To estimate the concrete security of lattice-based schemes, the [Lattice Estimator](https://github.com/malb/lattice-estimator) is the standard tool, and [fplll](https://github.com/fplll/fplll) and [G6K](https://github.com/fplll/g6k) provide state-of-the-art implementations of lattice reduction and sieving for experiments. As the number of new lattice assumptions grows, Martin Albrecht's overview of [SIS with hints](https://malb.io/sis-with-hints.html) is a useful reference: it catalogues SIS-like assumptions that give out additional hints, and records whether each is known to be standard, equivalent to another assumption, or broken.
