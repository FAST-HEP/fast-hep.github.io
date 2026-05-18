---
title: "Workflow language"
weight: 1
---

FAST-HEP workflows are described declaratively using YAML files.

Rather than writing analysis execution logic directly in Python scripts, users describe:

- datasets
- data sources
- transformations
- selections
- histograms
- rendering
- outputs
- execution configuration

The FAST-HEP ecosystem then compiles and executes the workflow.

The primary entry point is typically: `author.yaml`


---

## Goals

The workflow language aims to make analyses:

- readable
- reproducible
- composable
- inspectable
- portable across execution backends
- easier to validate and debug

The language intentionally separates:

- *what* should happen
- from *how* execution happens internally

---

## Design philosophy

The workflow language intentionally favors:

- explicit structure
- inspectable configuration
- composable extensions
- stable interfaces
- modular ownership

FAST-HEP separates:

- workflow orchestration (`fasthep-flow`)
- analysis transforms (`fasthep-carpenter`)
- metadata/provenance (`fasthep-curator`)
- rendering (`fasthep-render`)

This separation allows analyses to evolve while keeping workflow infrastructure relatively stable.

---

## Workflow compilation

A workflow is not executed directly from the raw YAML.

Instead, FAST-HEP performs several stages:

```mermaid
flowchart LR

    Author["author.yaml"]
    Normalised["normalised workflow"]
    Plan["execution plan"]
    Runtime["runtime execution"]
    Artifacts["artifacts and outputs"]

    Author --> Normalised
    Normalised --> Plan
    Plan --> Runtime
    Runtime --> Artifacts
```

These stages help provide:

- validation
- dependency inference
- backend abstraction
- debugging support
- reproducibility artifacts

The compilation pipeline is described further in:

- `author-normalise-plan`

---

## High-level structure

A FAST-HEP workflow is usually written as an `author.yaml` file.

The exact contents depend on the analysis, but most workflows follow the same broad shape:

```yaml
version: 1.0

use:
  profiles:
    - registry
    - fasthep_carpenter:registry
    - fasthep_curator:registry
    - fasthep_render:registry
    - fasthep_workshop:registry

data:
  datasets:
    - name: data
      eventtype: data
      files:
        - data/CMS/Zmumu/data.root
    - name: dy
      files:
        - data/CMS/Zmumu/dy.root
  defaults:
    eventtype: mc
    tree_primary: events

sources:
  events:
    kind: root_tree
    tree: events
    stream_type: event_stream

styles:
  dimuon_mass:
    op: hep.render.data_mc
    axes:
      x:
        name: dimu_mass
        label: 'Di-muon Mass, $M_{\mu\mu}$ [GeV/c$^{2}]$'
        limits: [60, 120]
      y:
        name: events
        label: Events
        scale: log
    data_mc:
      data: data
      signals: [dy]
      backgrounds: [qcd, single_top, ttbar, wjets, ww, wz, zz]
      stack: true
      ratio: true

observers:
  - kind: hep.schema_snapshot
    at: [read.events, stage.BasicVars]
    out: schema

analysis:
  stages:
    - id: BasicVars
      op: hep.define
      params:
        variables:
          - name: Muon_Pt
            expr: "sqrt(Muon_Px ** 2 + Muon_Py ** 2)"
          - name: IsoMuon_Idx
            expr: "(Muon_Iso / Muon_Pt) < 0.10"
          - name: NIsoMuon
            reduce:
              op: count_nonzero
              over: IsoMuon_Idx

    - id: DiMuons
      op: hep.di_object_mass
      params:
        collection: Muon
        mask: IsoMuon_Idx

    - id: DiMuonMass
      op: hep.hist
      params:
        dataset_axis: true
        axes:
          - name: dimu_mass
            source: DiMuon_Mass
            type: regular
            bins:
              low: 60
              high: 120
              nbins: 60
        weight_expr: EventWeight
      render:
        style: dimuon_mass
        when: final
```

This example describes:

1. which extension profiles are active
2. which datasets are available
3. how event data are read
4. how derived variables are calculated
5. how physics objects are combined
6. how histograms are filled
7. how plots are rendered
8. which diagnostics are collected

The YAML describes the analysis intent. FAST-HEP then turns this into an executable plan.

---

## Declarative execution

FAST-HEP workflows describe analysis intent rather than execution mechanics.

For example:

```yaml
- id: BasicVars
  op: hep.define
  params:
    variables:
      - name: Muon_Pt
        expr: "sqrt(Muon_Px ** 2 + Muon_Py ** 2)"
```

This says:

- create a new variable called `Muon_Pt`
- compute it from `Muon_Px` and `Muon_Py`
- use the operation registered as `hep.define`

It does **not** say:

- how to loop over events
- how to split data into partitions
- whether execution is local or distributed
- which backend schedules the work
- whether the calculation runs on CPU or, in future, GPU

From this declaration, FAST-HEP can infer that the operation needs the input fields:

```text
Muon_Px
Muon_Py
```

and produces the output field: `Muon_Pt`

That dependency information is used during compilation and planning. The same workflow description can then be executed by different backends without changing the analysis logic.

---

## Profiles and extensions

The workflow language is extensible through profiles.

Profiles register additional:

- operations
- sources
- sinks
- hooks
- renderers
- runtime extensions

Example:

```yaml
use:
  profiles:
    - fasthep_carpenter:registry
    - fasthep_render:registry
```

Profiles are typically provided by installable Python packages.

This enables analyses and experiments to extend FAST-HEP without modifying core workflow infrastructure.

---


## Sources and streams

Data are introduced into workflows through sources.

Examples include:

- ROOT TTrees
- awkward-array based sources
- toy/example generators
- experiment-specific readers

Sources produce streams of structured event data.

Example:

```yaml
sources:
  events:
    kind: root_tree
    tree: Events
```

Sources are generally implemented in:

- `fasthep-carpenter`
- experiment-specific extension packages

---

## Operations

Operations transform data or produce derived artifacts.

Each operation is identified by an operation kind, e.g. `op: hep.define`

Operations are registered through profiles and can be provided by different packages.

### Derived variables

Example:

```yaml
- id: BasicVars
  op: hep.define
  params:
    variables:
      - name: Muon_Pt
        expr: "sqrt(Muon_Px ** 2 + Muon_Py ** 2)"
```

This operation derives a new field from existing event data.

### Histogramming

Example:

```yaml
- id: DiMuonMass
  op: hep.hist
  params:
    axes:
      - name: dimu_mass
        source: DiMuon_Mass
        type: regular
        bins:
          low: 60
          high: 120
          nbins: 60
```

This operation fills a histogram from event-level quantities.

### Rendering

Rendering is usually attached declaratively to produced artifacts:

```yaml
render:
  style: dimuon_mass
  when: final
```

The rendering backend is resolved through registered render operations and styles.

### Writing outputs

Some operations or sinks may persist outputs to disk.

Example use cases include:

- ROOT output files
- parquet datasets
- JSON summaries
- plots and reports

FAST-HEP attempts to keep artifact generation explicit and inspectable.

---
## Observers and diagnostics

Observers inspect workflow execution and generated data products.

Example:

```yaml
observers:
  - kind: hep.schema_snapshot
    at: [read.events, stage.BasicVars]
    out: schema
```

This observer captures schema information during execution.

Observers are commonly used for:

- diagnostics
- provenance
- debugging
- schema validation
- runtime summaries

Observers generally do not modify event data directly.

---

## Hooks and lifecycle events

Hooks allow workflows and extensions to react to lifecycle events during execution.

Unlike normal operations, hooks are typically triggered by execution phases rather than appearing directly as analysis stages.

Examples include:

- partition-level hooks
- dataset-level hooks
- finalization hooks
- runtime diagnostics
- summary generation

Hooks are commonly used for:

- logging
- metrics
- diagnostics
- metadata collection
- artifact aggregation

This separation helps keep analysis logic distinct from runtime orchestration concerns.

---

## Outputs and artifacts

Workflows may produce:

- histograms
- plots
- reports
- metadata
- diagnostics
- provenance snapshots
- intermediate artifacts

Generated artifacts are typically written into a build directory.

FAST-HEP attempts to make execution outputs inspectable and reproducible.

---

## Execution backends

The workflow language is designed to remain largely independent of execution backend details.

Possible backends include:

- local execution
- Dask/distributed execution
- future batch/grid execution systems

Backends are responsible for execution orchestration rather than workflow semantics.

---


## Next steps

Related concept pages:

- [Workflow compilation pipeline]({{< ref "author-normalise-plan.md" >}})
- [Operations and specs]({{< ref "operations-and-specs.md" >}})
- [Profiles and registries]({{< ref "profiles-and-registries.md" >}})
- [Execution backends]({{< ref "execution-backends.md" >}})
- [Analysis repositories]({{< ref "analysis-repositories.md" >}})
