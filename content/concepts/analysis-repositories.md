---

title: "Analysis repositories"
weight: 7
---

FAST-HEP encourages analysis-specific Python code to live in an installable
Python package.

An analysis repository can then bring together:

- workflow descriptions
- analysis-specific operations
- profiles and registries
- configuration
- tests
- documentation

while giving the analysis-specific code a clear identity as a versioned
software component.

```mermaid
flowchart LR
    Repository["<b>Analysis repository</b>"]:::input
    Workflow["<b>workflow.yaml</b><br/>scientific workflow"]:::input
    Package["<b>Python package</b><br/>analysis capabilities"]:::capability
    Profile["<b>Analysis profile</b><br/>compose environment"]:::capability
    Flow["<b>Flow</b><br/>compile + execute"]:::flow

    Repository --> Workflow
    Repository --> Package
    Package --> Profile
    Workflow --> Flow
    Profile --> Flow
```

This is particularly important for reproducibility: analysis-specific code is part of the software environment that produced a scientific result.

---

## Recommended structure

A FAST-HEP analysis might use a structure such as:

```text
my-analysis/
├── workflow.yaml
├── pyproject.toml
├── README.md
├── src/
│   └── my_analysis/
│       ├── profiles/
│       │   └── registry.yaml
│       ├── operations/
│       └── __init__.py
└── tests/
```

The exact structure is not prescribed. An analysis only needs the components relevant to it.

The important recommendation is that analysis-specific Python code lives in an **installable package** rather than relying on scripts or directories added manually to `PYTHONPATH`.

The package can be installed normally with tools such as `pip` or `pixi`, including as an editable installation during development.

---

## Analysis-specific capabilities

Many analyses need functionality beyond the operations provided by the standard FAST-HEP toolkit.

For example, an analysis may provide:

* experiment-specific calculations
* specialised object definitions or selections
* custom data readers
* alternative implementations of existing operations
* specialised output handling
* additional diagnostics

These capabilities can use the same registry and profile mechanisms as the rest of FAST-HEP.

```mermaid
flowchart TD
    HEP["<b>HEP profile</b><br/>FAST-HEP capabilities"]:::capability
    Experiment["<b>Experiment profile</b><br/>experiment capabilities"]:::capability
    Analysis["<b>Analysis profile</b><br/>analysis-specific capabilities"]:::capability

    HEP --> Experiment --> Analysis
```

An analysis can therefore extend an existing environment rather than modifying Flow or the FAST-HEP packages that it depends on.

See [Profiles and registries]({{< ref "profiles-and-registries.md" >}}) for how these environments are composed.

---

## Why installable packages?

Python allows code to be made importable simply by manipulating `PYTHONPATH`. This is common in existing analysis environments, but provides little information about where that code came from or which version was used.

Installing analysis code as a package gives it a clearer identity within the software environment.

This makes it easier to:

* declare and reproduce dependencies
* identify analysis-specific software
* record package versions
* test code independently
* use standard Python development tooling
* share the analysis with collaborators

For FAST-HEP, this also supports provenance.

Current compile bundles record installed distributions, editable-checkout
revisions and dirty state, lock-file evidence, and the distributions that
provided packaged registry or profile layers. The plan retains the resolved
registry and implementation references used by the runtime.

This already captures substantially more than the authored workflow alone.
Collecting equivalent evidence from every distributed worker and external
resource remains an area of ongoing development.

---

## Reproducibility

A reproducible analysis requires more than preserving its `workflow.yaml`.

The workflow may refer to operations provided by:

```mermaid
flowchart TD
    FASTHEP["<b>FAST-HEP packages</b>"]:::capability
    Experiment["<b>Experiment packages</b>"]:::capability
    Analysis["<b>Analysis package</b>"]:::capability
    Environment["<b>Executable scientific environment</b>"]:::runtime

    FASTHEP --> Environment
    Experiment --> Environment
    Analysis --> Environment
```

Together, these packages form part of the executable scientific environment.

Because FAST-HEP deliberately allows implementations to be replaced, preserving
the workflow alone is insufficient for reproducibility: it is also necessary
to know **which implementations and software versions were actually used**.

---

## Testing and development

Treating an analysis as a software package also makes conventional software-engineering practices easier to adopt.

Analysis repositories can include:

* unit tests for analysis-specific operations
* workflow smoke tests
* regression tests for scientific outputs
* integration tests
* documentation and examples

None of these require special FAST-HEP mechanisms; standard Python tooling can be used alongside the workflow.

---

## Examples and guidance

The [`fasthep-workshop`](https://github.com/FAST-HEP/fasthep-workshop) repository contains runnable analyses and examples of FAST-HEP project organisation.

The recommended repository structure will continue to evolve as FAST-HEP tooling and provenance support mature.

The aim is not to impose a rigid directory layout, but to make analysis-specific software **installable, identifiable, testable, and reproducible**.

---

## Related concepts

* [Workflow language]({{< ref "workflow-language.md" >}})
* [Compilation and execution]({{< ref "compilation-and-execution.md" >}})
* [Operations and specs]({{< ref "operations-and-specs.md" >}})
* [Profiles and registries]({{< ref "profiles-and-registries.md" >}})
* [Execution environments]({{< ref "execution-environments.md" >}})
