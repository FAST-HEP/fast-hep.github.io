---

title: "Profiles and registries"
weight: 4
---

FAST-HEP keeps capabilities separate from the workflow engine.

**Registries** describe the capabilities available to Flow and how they are
resolved.

**Profiles** compose registries, compiler extensions, and runtime configuration
into reusable environments.

```mermaid
flowchart LR
    Packages["<b>Packages</b><br/>provide capabilities"]:::capability
    Registry["<b>Registries</b><br/>describe + resolve"]:::flow
    Profile["<b>Profiles</b><br/>compose environment"]:::input
    Flow["<b>Flow</b><br/>compile + execute"]:::runtime

    Packages --> Registry --> Profile --> Flow
```

A workflow selects an environment through this mechanism without needing to
know which packages provide each individual capability.

Together, registries and profiles provide useful defaults while keeping
capabilities replaceable and independently extensible.

---

## Registries

A registry is a catalogue of named capabilities available to Flow. Registry
entries provide the information needed to resolve those capabilities during
compilation and execution.

The [Carpenter registry](https://github.com/FAST-HEP/fasthep-carpenter/blob/main/src/fasthep_carpenter/profiles/registry.yaml),
for example, provides capabilities including data sources, transforms, sinks, execution modifiers, product handling, and expression functions.

For operations, a registry associates a workflow-visible name with its specification and runtime implementation:

```mermaid
flowchart LR
    Name["<b>hep.define</b><br/>workflow-visible name"]:::transform
    Spec["<b>Spec</b><br/>compile-time contract"]:::flow
    Impl["<b>Implementation</b><br/>runtime behaviour"]:::runtime

    Name --> Spec
    Name --> Impl
```

The real Carpenter registry contains entries such as `hep.define`, `hep.hist`, and `hep.di_object_mass`, alongside capabilities for ROOT I/O, GPU execution modifiers, product handling, and expression functions.

This is how Flow can orchestrate capabilities supplied by other packages while remaining independent of their internal implementation.

---

## Profiles and composition

Profiles provide reusable combinations of capabilities and configuration.

For example, the
[`hep`](https://github.com/FAST-HEP/fasthep-flow/blob/main/src/hepflow/profiles/hep.yaml)
profile combines the capabilities needed for a typical HEP analysis, while the [`hep_debug` profile](https://github.com/FAST-HEP/fasthep-flow/blob/main/src/hepflow/profiles/hep_debug.yaml) builds on it by adding runtime diagnostics.

Profiles can include other profiles, allowing environments to be built through composition:

```mermaid
flowchart TD
    Basic["<b>basic</b><br/>Flow environment"]:::flow
    HEP["<b>hep</b><br/>HEP capabilities"]:::capability
    Debug["<b>hep_debug</b><br/>runtime diagnostics"]:::runtime
    Experiment["<b>experiment</b><br/>experiment capabilities"]:::capability
    Analysis["<b>analysis</b><br/>analysis-specific capabilities"]:::capability

    Basic --> HEP
    HEP --> Debug
    HEP --> Experiment
    Experiment --> Analysis
```

An analysis can therefore select a useful environment without needing to know which individual packages and registries provide every capability.

The same composition mechanism is available to experiments and individual analyses. They can add new capabilities or replace existing implementations while continuing to build on the standard FAST-HEP environment.

This allows customisation through composition rather than by modifying Flow or FAST-HEP packages directly.

{{< admonition note >}}
**Provenance and reproducibility:** FAST-HEP is being extended to record resolved
capabilities, concrete implementations, and provider package versions in the
workflow provenance. This will record not only what an analysis requested, but
which software components actually executed it.

This support is currently under development.
{{< /admonition >}}


---

## Extending FAST-HEP

Registries and profiles are not limited to packages maintained by FAST-HEP.

Experiments, projects, and individual analyses can provide their own capabilities using the same interfaces as the standard toolkit.

For example, an extension package might provide:

* experiment-specific data readers
* specialised analysis operations
* alternative implementations of existing operations
* GPU-oriented runtime behaviour
* custom product handling
* additional diagnostics

Those capabilities can then be composed with existing FAST-HEP profiles.

```mermaid
flowchart LR
    Flow["<b>Flow</b><br/>core environment"]:::flow
    HEP["<b>FAST-HEP</b><br/>HEP capabilities"]:::capability
    Experiment["<b>Experiment</b><br/>custom capabilities"]:::capability
    Analysis["<b>Analysis</b><br/>local capabilities"]:::capability

    Flow -->|compose| HEP
    HEP -->|extend| Experiment
    Experiment -->|extend| Analysis
```

Flow does not need to distinguish between a capability supplied by FAST-HEP and one supplied by an external package. What matters is that it satisfies the relevant contract.

---

## Why this matters

Profiles and registries make extensibility part of the FAST-HEP architecture rather than an additional plugin mechanism layered on top of it.

Flow provides the compilation and runtime machinery, while the capabilities that interact with scientific data can come from FAST-HEP, experiments, external projects, or individual analyses.

Combined with the operation contracts described in [Operations and specs]({{< ref "operations-and-specs.md" >}}), this allows implementations to evolve or be replaced without requiring the workflow engine itself to understand their internal details.

---

## Learn more

This page describes the role of profiles and registries in the FAST-HEP architecture.

For the current profile format, registry schema, composition rules, resource
discovery, overrides, and extension interfaces, see the
[fasthep-flow documentation](https://fasthep-flow.readthedocs.io/en/latest/).

### Related concepts

* [Workflow language]({{< ref "workflow-language.md" >}})
* [Compilation and execution]({{< ref "compilation-and-execution.md" >}})
* [Operations and specs]({{< ref "operations-and-specs.md" >}})
* [Execution environments]({{< ref "execution-environments.md" >}})
* [Analysis repositories]({{< ref "analysis-repositories.md" >}})
