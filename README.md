<p align="center">
  <a href="https://github.com/TrentBest/TheSingularityWorkshop.Visualize">
    <img src="https://raw.githubusercontent.com/TrentBest/TheSingularityWorkshop.Visualize/development/docs/assets/moniker.svg" alt="The Singularity Workshop" width="720" />
  </a>
</p>

# TheSingularityWorkshop.Benchmarks.NeuralNetworks

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Last commit](https://img.shields.io/github/last-commit/TrentBest/TheSingularityWorkshop.Benchmarks.NeuralNetworks/development)](https://github.com/TrentBest/TheSingularityWorkshop.Benchmarks.NeuralNetworks/commits/development)

[![Sith Policy](assets/sith-policy.svg)](https://github.com/TrentBest/TheSingularityWorkshop.Benchmarks.NeuralNetworks/actions/workflows/sith-policy.yml)

**Status: benchmark contract only — no executable neural-network implementation is currently available to measure.**

## 00 Identity

This repository is the performance laboratory for TheSingularityWorkshop.FSM_NeuralNetworks. It exists to measure actual neural computation, backend selection, memory movement, and execution costs—not to publish synthetic numbers from placeholder code.

## 01 What

The source repository currently contains architecture and MVP contract documentation, marked proposed; it does not yet expose an executable model, tensor, evaluator, or backend implementation. Therefore this repository intentionally contains no timing benchmark and reports no performance baseline.

The planned first benchmark target is the CPU reference inference path using the exact two-input/two-output ReLU acceptance fixture in the source architecture decisions.

## 02 Why

A benchmark is useful only when it measures the implementation and semantics that consumers will execute. Inventing a local tensor loop or mock GPU backend would measure the benchmark author's substitute, not NeuralNetworks.

The CPU reference must establish correctness first. GPU measurements must later include dispatch, transfer, synchronization, allocation, and actual backend identity; a silent CPU fallback must never be reported as GPU execution.

## 03 How

The benchmark contract is recorded in [BENCHMARK_CONTRACT.md](docs/BENCHMARK_CONTRACT.md). It defines:
- correctness gates and the shared acceptance fixture;
- model sizes and workload families to add only when supported by real APIs;
- CPU reference versus accelerated-backend measurement boundaries;
- transfer, dispatch, synchronization, allocation, and trace costs;
- provenance, environment capture, and fail-closed result interpretation.

No CI benchmark job is enabled until there is executable source to build and measure. Once the first implementation exists, this repository will add a project reference pinned to a known source revision and a Dry smoke gate before any full measurement profile.

## 04 See it in a minute

Read the current source contracts:
- [NeuralNetworks architecture](https://github.com/TrentBest/TheSingularityWorkshop.FSM_NeuralNetworks/blob/development/Documentation/Architecture/ARCHITECTURE.md)
- [MVP contract decisions](https://github.com/TrentBest/TheSingularityWorkshop.FSM_NeuralNetworks/blob/development/Documentation/Architecture/MVP_CONTRACT_DECISIONS.md)
- [Benchmark contract](docs/BENCHMARK_CONTRACT.md)

The first executable benchmark will measure the exact shared acceptance fixture and verify its expected output before timing. Larger cases will be introduced only after the implementation defines and tests the corresponding operations.

## Evidence policy

- No fabricated benchmark values or inferred GPU capability.
- Correctness and performance are separate gates.
- Backend identity and fallback behavior must be observable.
- Record both source and benchmark commit SHAs, runtime, OS, CPU, device, driver, backend, precision, and workload.
- Separate model evaluation from host/device transfer, dispatch, synchronization, and trace capture.
- No result authorizes package publication, merge, deployment, or release.

<p align="center"><img src="https://raw.githubusercontent.com/TrentBest/FSM_API/master/Documentation/Branding/TheSingularityWorkshop.png" alt="The Singularity Workshop" width="200" /></p>
