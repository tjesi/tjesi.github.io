---
title: "Lattices"
permalink: /lattices/
---

Lattice-based cryptography is the leading approach to post-quantum cryptography. This page gives a short introduction to the area and points to a small selection of resources that I recommend to students and colleagues who want to learn it.

## What is a lattice?

A lattice is a regular grid of points in *n*-dimensional space. Given linearly independent basis vectors **b**<sub>1</sub>, …, **b**<sub>n</sub>, the lattice consists of all their integer linear combinations *z*<sub>1</sub>**b**<sub>1</sub> + … + *z*<sub>n</sub>**b**<sub>n</sub>. The same lattice has infinitely many bases: a "good" basis of short, nearly orthogonal vectors makes many problems easy, while a "bad" basis of long, skewed vectors hides the structure of the lattice. Finding a short non-zero vector (the Shortest Vector Problem, SVP) or the lattice point closest to a given target (the Closest Vector Problem, CVP) is believed to be hard in high dimensions, for classical and quantum computers alike. The best known algorithms, lattice reduction such as BKZ combined with sieving or enumeration, take time exponential in the dimension.

## Why lattices for cryptography?

Modern lattice cryptography is built on two average-case problems. The Short Integer Solution (SIS) problem asks for a short non-zero integer vector **x** with **Ax** = **0** mod *q* for a random matrix **A**, and the Learning with Errors (LWE) problem asks to recover a secret **s** from noisy linear equations **b** = **As** + **e** mod *q*. Starting with the work of Ajtai (1996) and Regev (2005), both problems have been shown to be at least as hard as worst-case lattice problems (for LWE, originally via a quantum reduction), which gives unusually strong theoretical foundations. Structured variants over polynomial rings (Ring-LWE and Module-LWE/SIS) reduce key sizes and computation dramatically, and are what make lattice schemes practical.

No quantum algorithm is known to solve these problems significantly faster than classical ones, in contrast to RSA and elliptic-curve cryptography, which are broken by Shor's algorithm. In August 2024, NIST published its first post-quantum standards: [ML-KEM](https://csrc.nist.gov/pubs/fips/203/final) (FIPS 203, based on Kyber) for key encapsulation and [ML-DSA](https://csrc.nist.gov/pubs/fips/204/final) (FIPS 204, based on Dilithium) for digital signatures, both built on module lattices. The [NIST Post-Quantum Cryptography project](https://csrc.nist.gov/projects/post-quantum-cryptography) tracks the remaining standardization work.

Beyond encryption and signatures, lattices are remarkably versatile. They underlie all practical fully homomorphic encryption schemes, and support advanced primitives such as zero-knowledge proofs, threshold and blind signatures, verifiable encryption and mix-nets, and identity- and attribute-based encryption, which is where much of my own [research](/research/) is focused.

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

To estimate the concrete security of lattice-based schemes, the [Lattice Estimator](https://github.com/malb/lattice-estimator) is the standard tool, and [fplll](https://github.com/fplll/fplll) and [G6K](https://github.com/fplll/g6k) provide state-of-the-art implementations of lattice reduction and sieving for experiments. As the number of new lattice assumptions grows, Martin Albrecht's overview of [SIS with hints](https://malb.io/sis-with-hints.html) is a useful reference: it catalogues SIS-like assumptions that give out additional hints, and records whether each is known to be standard, equivalent to another assumption, or broken. His [website](https://malb.io) links to more of his software and resources on lattice cryptanalysis.
