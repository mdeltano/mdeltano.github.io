+++
title = "Zero-Divisor Graph Analysis Algorithm"
description = "Custom Mathematica algorithm that delivered a ~100× speedup over prior approaches, enabling analysis of graphs with 1,000+ nodes. Contributed to a peer-reviewed publication in Graphs and Combinatorics."
weight = 2
date = 2023-12-01

[extra]
# local_image = "zero-divisor.png"
tags = ["mathematica", "algorithms", "research", "performance"]
+++

## Overview

As a research assistant in Florida Polytechnic's computational
mathematics group, I designed and implemented a single-threaded
algorithm from scratch in Mathematica to analyze **zero-divisor
graphs** — algebraic structures whose analysis had previously been done
manually or with ad-hoc tooling, making large-scale study impractical.

## My role

I owned the algorithm end-to-end: design, implementation,
benchmarking, and the technical writeup that fed into a peer-reviewed
publication.

## How it works

The core insight was combining **targeted pruning logic** (eliminating
non-viable candidates before full evaluation) with **early-exit
brute-force checks**, so most expensive comparisons never run. To keep
memory bounded on large inputs, the algorithm stores only verified
results rather than intermediate state — important when working with
graphs at the 1,000+ node scale.

Numerous mathematical edge cases (self-comparisons, degenerate graph
structures) are handled by correctness checks embedded directly in the
algorithmic flow rather than as a separate validation pass.

## Outcome

- **~100× performance improvement** over prior approaches, reducing
  analyses that would have taken hundreds to thousands of times longer
  manually to tractable computational runtimes.
- **Adopted within the department** for continued use in ongoing
  research.
- **Contributed to a peer-reviewed publication** in [*Graphs and
  Combinatorics*](https://doi.org/10.1007/s00373-025-02944-3).
- **Presented** methodology, tradeoffs, and performance results to an
  external academic audience of 5,000+ students and faculty.

This project sharpened how I reason about algorithmic tradeoffs — speed
vs. memory, generality vs. specialization — under real time
constraints, and I can walk through the design decisions in depth.
