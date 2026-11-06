# NeuralNetworks Benchmark Contract

**Status:** Planned measurement contract; no executable benchmarks yet.  
**Source:** TheSingularityWorkshop.FSM_NeuralNetworks development branch.  
**Authority:** The source architecture and MVP contract remain authoritative for semantics.

## 1. Entry gate

Do not create timed benchmarks until the source repository contains a buildable, tested implementation of:
- logical tensor and element-type contracts;
- validated model graph and immutable parameter version;
- CPU reference inference;
- the MVP Input → Dense → ReLU operation path;
- execution result metadata identifying the backend;
- explicit bounded execution behavior.

The first benchmark project must reference the actual source project. It must not reimplement the evaluator inside this repository.

## 2. Correctness fixture

Reuse the source repository's exact 2-input / 2-output dense-ReLU acceptance fixture:

- Input x = [2, 1]
- Weights W = [[2, 1], [-1, 3]], rows are output units
- Bias b = [-1, 2]
- Affine result z = [4, 3]
- ReLU output y = [4, 3]

Before timing, assert output shape, element type, exact CPU reference values, model-version identity, and trace/result consistency when trace capture is enabled. The fixture is intentionally small and hand-checkable; it is a correctness gate, not a throughput benchmark.

## 3. Workload families (future, implementation-gated)

| Family | Initial dimensions | Measurement question |
|---|---|---|
| Tiny inference | Shared 2×2 acceptance fixture | Fixed per-request overhead and allocation |
| Dense feed-forward | Input/output widths 8, 32, 128, 512; 1, 2, 4, 8 layers | CPU reference scaling with operations and parameter count |
| Batch inference | Batch sizes 1, 8, 32, 128 | Amortization and throughput |
| Precision | Only types explicitly implemented and declared by a backend | Accuracy/performance trade-off |
| Trace | Disabled, summary, bounded full trace | Cost of inspectability |
| Parameter versioning | Stable model version vs creation of a new version | Snapshot and update costs |

These are candidate dimensions, not a claim that the current source supports them. A workload must be omitted until its operation, shape, precision, and ownership contracts exist.

## 4. CPU and GPU boundaries

For CPU reference:
- measure model evaluation separately from model construction and validation;
- measure warm inference and cold initialization separately;
- include allocation diagnostics;
- retain a checksum/result consumer and validate correctness outside the timed method.

For an accelerated backend, report separate measurements for:
1. backend/device discovery and selection;
2. host-to-device transfer;
3. parameter upload or cache reuse;
4. dispatch/launch overhead;
5. device execution;
6. synchronization/completion wait;
7. device-to-host result transfer;
8. result materialization;
9. optional trace extraction.

Also report end-to-end request latency. Do not sum independently measured phases and present the sum as an end-to-end result unless the execution path and overlap semantics justify that calculation.

A backend requested as required must fail explicitly if unavailable. Never count CPU fallback as GPU execution. Record actual backend, device, driver/runtime, precision, and fallback decision in every result.

## 5. Scaling and memory

When real operations support the dimensions, measure:
- parameter count and model topology;
- tensor element count and batch size;
- allocation count and bytes;
- host and device memory separately;
- transfer volume;
- warm/cold cache behavior;
- concurrency only after synchronization and ownership semantics are tested.

Start with small and medium dimensions. Do not begin with huge networks or million-item batches before validating the baseline and resource limits.

## 6. Statistical and provenance requirements

- Use BenchmarkDotNet for CPU microbenchmarks where its execution model matches the workload.
- Use a dedicated backend harness for asynchronous device timing; host wall-clock timing alone is not device execution time.
- Capture raw output and machine-readable summaries.
- Record benchmark commit SHA and exact source commit SHA.
- Record OS, CPU, RAM, .NET SDK/runtime, backend, device, driver, precision, and workload parameters.
- Compare like-for-like environments and retain repeated independent runs.
- No single run creates a regression threshold or performance verdict without a justified comparison policy.

## 7. First implementation sequence

1. Source repository lands the CPU inference MVP and conformance tests.
2. This repository adds a project reference pinned to that source commit.
3. Build and correctness gate execute in CI.
4. A BenchmarkDotNet Dry smoke job proves the harness executes.
5. A measured CPU baseline captures tiny and dense feed-forward workloads.
6. Backend selection and capability contracts are implemented and tested.
7. Hardware reconnaissance selects an actual target backend.
8. Accelerated benchmarks measure the full transfer/dispatch/compute/sync lifecycle.

NeuralNetworks provides bounded computation and inspectable results. It does not own goals, permissions, planning, or action authority; those remain with the consuming system.
