---
title: "Execution backends"
weight: 5
---

FAST-HEP separates workflow definition from workflow execution.

The workflow language describes:

- what data should be processed
- which operations should run
- which artifacts should be produced

Execution backends determine:

- where execution happens
- how tasks are scheduled
- how data are partitioned
- how resources are allocated

This separation allows the same workflow to run across different execution environments without changing the analysis definition itself.

---

## Current execution model

The current FAST-HEP alpha implementation primarily targets:

- local execution
- Dask-based distributed execution

Execution planning currently focuses on:

- partitioned event processing
- dependency-aware scheduling
- artifact generation
- runtime diagnostics

The backend layer is still evolving as the ecosystem stabilises.

---

## Local execution

Local execution is intended for:

- development
- debugging
- tutorials
- small analyses

Typical usage:

```bash
fasthep run author.yaml
```

The workflow is compiled, planned, and executed locally without requiring external schedulers.

---

## Distributed execution

FAST-HEP is designed to support distributed execution through interchangeable backends.

The primary near-term distributed target is Dask.

Planned and experimental integrations include:

| Backend | Purpose |
|---|---|
| `dask:local` | local distributed execution |
| `dask:htcondor` | HTCondor-backed worker scaling |
| `dask:dirac` | DIRAC/grid-integrated execution |

These integrations are still evolving and should currently be considered experimental or planned functionality.

---

## Strategies

Strategies are a planned mechanism for injecting backend-specific execution behavior into workflows without modifying the workflow definition itself.

The goal is to allow analyses to tune execution for specific environments while preserving workflow portability.

Possible use cases include:

- partition sizing
- scheduling preferences
- worker resource hints
- caching behavior
- dataset placement
- backend-specific optimisation hooks

Strategies are still under active design and should currently be considered work in progress.

{{< admonition note >}}
Strategies are intended to customise execution behavior rather than redefine workflow semantics. The analysis workflow should remain portable across backends where practical.
{{< /admonition >}}

---

## Future execution targets

Potential future execution targets include:

- GPU-aware execution
- heterogeneous CPU/GPU scheduling
- Snakemake integration
- experiment-specific batch systems
- cloud-native execution environments

At present, FAST-HEP focuses primarily on internal workflow planning and runtime orchestration.

---

## Planning versus execution

FAST-HEP intentionally distinguishes between:

- workflow planning
- workflow execution

The planner determines:

- operation ordering
- dependencies
- execution phases

The backend determines:

- where execution occurs
- how work is scheduled
- how tasks are parallelised

This distinction is central to the FAST-HEP architecture.

---

## Related concepts

- [Workflow language]({{< ref "workflow-language.md" >}})
- [Workflow compilation pipeline]({{< ref "author-normalise-plan.md" >}})
- [Operations and specs]({{< ref "operations-and-specs.md" >}})
- [Profiles and registries]({{< ref "profiles-and-registries.md" >}})
- [Analysis repositories]({{< ref "analysis-repositories.md" >}})
