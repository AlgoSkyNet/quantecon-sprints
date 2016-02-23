---
title:  'February 23, 2016'
subtitle: "QuantEcon.jl Markov chains"
author:
- name: "Spencer Lyon"
  affiliation: "New York University"
  email: "spencer.lyon@stern.nyu.edu"
date: "2016-02-22"
---

Today we will be working on improving the support for dealing with Markov
chains and discrete decision processes in QuantEcon.jl

## Agenda

- Discuss what the code sprints are supposed to accomplish (5 min)
- Discuss topics for the next code sprint (5 min. Matt McKay)
- Overview of the day's goals (10 min. Spencer Lyon)
- Sprint! (rest of the time)

## Goals

The goals for this code sprint are mostly items that will bring QuantEcon.jl
up to equal grounds with QuantEcon.py for this type of work.

Here are some tasks we'd like to accomplish before we are done:

- [ ] Support for sparse matrices in the `MarkovChain` type (ref [issue 97](https://github.com/QuantEcon/QuantEcon.jl/issues/97))
- [ ] Decide if we want to keep the `eigen` and `lu` options for `mc_compute_stationary`. It doesn't hurt to have them, but I believe they are strictly inferior to the `gth` method (ref [issue 75](https://github.com/QuantEcon/QuantEcon.jl/issues/75)). We should do accuracy and performance tests to verify that `gth` is better for matrices of all datatypes and sizes.
- [ ] Speed up the `MarkovChain` constructor. I think this will amount to devectorizing [this line](https://github.com/QuantEcon/QuantEcon.jl/blob/3001af73a1402e39b12cf9f6e80b66304805a116/src/mc_tools.jl#L63) into the proper loop.
- [ ] Decide if we should add a second field to the `MarkovChain` that holds the values of the states. This would default to the abstract vector `1:size(P, 1)`, where `P` is the stochastic matrix. I've [started this discussion before](https://github.com/QuantEcon/QuantEcon.jl/pull/20/files#r20576723), but I'd like to revisit is as I often wish I could have it.
- [ ] If the above point gets resolved in the affirmative, decide if we should have `tauchen` and `rouwenhorst` return instances of `MarkovChain` instead of the `(y, Π)` tuples.
- [ ] Move `mc_tools.jl` and `markov_approx.jl` to the `markov` folder  (ref [issue 84](https://github.com/QuantEcon/QuantEcon.jl/issues/84))
- [ ] Add more tests for the current ddp code (dense matrix version).
- [ ] Add tests for the yet to be finished sa-pair ddp code ([ref](https://github.com/QuantEcon/QuantEcon.jl/pull/94#issuecomment-174527895)). This should work off the `refactor-ddp` branch. We will develop against this in the next item
- [ ] Finish implementing the sa-pairs formulation. Much of this has already been done on the `refactor-ddp` branch, but should be reviewed
- [ ] Add sparse matrix support to ddp
- [ ] Implement `Base.show(io::IO, ddp::DiscreteDP)`
- [ ] Implement `QuantEcon.simulate(ddpr::DPSolveResult)` that just forwards to the `simulate(ddpr.mc)`. We could talk about simulating v and sigma also...
- [ ] Transpose the ddp code so that memory layout is optimal for column-major storage. This is potentially a rather big step. However, if we have done a good enough job on the preceding steps, we should have good enough tests in place to do this with confidence.
- [ ] (Optional) Write some performance benchmarks for `MarkovChain` methods, `tauchen` and `rouwenshort`, and the ddp routines. It would be awesome if someone wants to tackle this from the very beginning so we can do a before/after comparison from the day's sprint and measure our progress on this dimension.
