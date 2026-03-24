<div align="center">
<img src="assets/hero.svg" width="100%"/>
</div>

# agent-benchmark

**Performance benchmarking for LLM agents — timed runs, statistical summaries, regression detection.**

[![PyPI version](https://img.shields.io/pypi/v/agent-benchmark?color=yellow&style=flat-square)](https://pypi.org/project/agent-benchmark/) [![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue?style=flat-square)](https://python.org) [![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE) [![Tests](https://img.shields.io/badge/tests-passing-brightgreen?style=flat-square)](#)

---

## The Problem

Without benchmarks, performance regressions ship silently. A 40% latency increase between versions is invisible until it hits production. Repeatable, baseline-aware benchmarks catch regressions before users do.

## Installation

```bash
pip install agent-benchmark
```

## Quick Start

```python
from agent_benchmark import BenchmarkResult, ComparisonResult

# Initialise
instance = BenchmarkResult(name="my_agent")

# Use
result = instance.run()
print(result)
```

## API Reference

### `BenchmarkResult`

```python
class BenchmarkResult:
    name: str
    def is_faster_than(self, other: "BenchmarkResult") -> bool:
        """Return True if this result has a lower mean than *other*."""
    def regression_vs(self, baseline: "BenchmarkResult", threshold: float = 0.10) -> bool:
        """Return True if this result is more than *threshold* (default 10%) slower than baseline."""
    def to_dict(self) -> dict[str, Any]:
    def summary(self) -> str:
```

### `ComparisonResult`

```python
class ComparisonResult:
    result_a: BenchmarkResul
```


## How It Works

### Flow

```mermaid
flowchart LR
    A[User Code] -->|create| B[BenchmarkResult]
    B -->|configure| C[ComparisonResult]
    C -->|execute| D{Success?}
    D -->|yes| E[Return Result]
    D -->|no| F[Error Handler]
    F --> G[Fallback / Retry]
    G --> C
```

### Sequence

```mermaid
sequenceDiagram
    participant App
    participant BenchmarkResult
    participant ComparisonResult

    App->>+BenchmarkResult: initialise()
    BenchmarkResult->>+ComparisonResult: configure()
    ComparisonResult-->>-BenchmarkResult: ready
    App->>+BenchmarkResult: run(context)
    BenchmarkResult->>+ComparisonResult: execute(context)
    ComparisonResult-->>-BenchmarkResult: result
    BenchmarkResult-->>-App: WorkflowResult
```

## Philosophy

> The Gita's *svadharma* demands we measure ourselves against our own standard, not another's; benchmarks are that mirror.

---

*Part of the [arsenal](https://github.com/darshjme/arsenal) — production stack for LLM agents.*

*Built by [Darshankumar Joshi](https://github.com/darshjme), Gujarat, India.*
