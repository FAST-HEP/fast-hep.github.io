---
title: "Analysis repositories"
weight: 6
---

FAST-HEP encourages analyses to be structured as installable Python packages rather than collections of standalone scripts.

This approach improves:

- reproducibility
- dependency management
- workflow portability
- reuse of custom transforms and hooks
- integration with FAST-HEP tooling

The recommended structure is intentionally similar to modern Python packaging practices.

---

## Recommended structure

A typical analysis repository may look like:

```text
my-analysis/
├── author.yaml
├── pyproject.toml
├── README.md
├── src/
│   └── my_analysis/
│       ├── profiles/
│       │   └── registry.yaml
│       ├── transforms/
│       ├── hooks/
│       ├── sinks/
│       ├── sources/
│       └── __init__.py
├── tests/
└── data/
```

This structure allows the analysis to behave like a normal Python package while also exposing FAST-HEP workflow extensions.

---

## Why package analyses?

Historically, many HEP analyses evolved as collections of scripts and notebooks.

FAST-HEP instead encourages analyses to expose:

- reusable transforms
- custom workflow operations
- experiment-specific sources
- rendering extensions
- diagnostics hooks

through standard Python packaging.

This makes analyses easier to:

- share
- validate
- test
- version
- distribute

---

## Registries and profiles

Analysis repositories typically expose a registry profile:

```text
src/my_analysis/profiles/registry.yaml
```

which can then be activated in workflows:

```yaml
use:
  profiles:
    - my_analysis:registry
```

This allows analyses to register:

- custom transforms
- hooks
- sinks
- sources
- rendering behavior

without modifying FAST-HEP core packages.

For more detail, see:

- [Profiles and registries]({{< ref "profiles-and-registries.md" >}})

---

## Transforms

Transforms contain reusable analysis logic.

Typical examples include:

- object definitions
- selections
- derived variables
- matching algorithms
- experiment-specific calculations

Example structure:

```text
transforms/
├── muons.py
├── jets.py
└── selections.py
```

Transforms are generally registered through the analysis registry.

---

## Sources

Sources introduce data into workflows.

Analysis repositories may provide:

- experiment-specific readers
- toy/example datasets
- cached intermediate formats
- derived datasets

Example:

```text
sources/
└── toy_source.py
```

---

## Hooks

Hooks allow analyses to react to runtime lifecycle events.

Common use cases include:

- diagnostics
- metadata collection
- summaries
- monitoring
- experiment-specific bookkeeping

Hooks intentionally remain separate from core analysis transforms.

---

## Sinks

Sinks consume produced artifacts and persist outputs.

Examples include:

- custom ROOT writers
- parquet exporters
- summary generators
- experiment-specific output formats

Rendering integrations are currently also implemented through sink-style interfaces.

---

## Testing

Analysis repositories are encouraged to include automated tests.

Typical testing layers include:

- unit tests for transforms
- workflow smoke tests
- rendering regression tests
- integration tests

FAST-HEP development tooling is designed to support cross-package integration testing through the `fasthep-dev` workspace.

---

## Workshop examples

The `fasthep-workshop` repository serves as a reference implementation of the recommended analysis structure.

It contains:

- tutorials
- example workflows
- custom sources
- example registries
- development patterns

The workshop repository is intended to evolve alongside the FAST-HEP ecosystem and demonstrate recommended practices.

---

## Future directions

Planned tooling may eventually include:

- `fasthep init`
- analysis templates
- registry scaffolding
- example generation
- backend configuration helpers

The goal is to make it easier to create portable and reproducible analysis repositories with minimal boilerplate.

---

## Design philosophy

FAST-HEP encourages analyses to behave like modular software projects rather than isolated execution scripts.

This helps support:

- long-term maintainability
- collaboration
- testing
- workflow reuse
- ecosystem interoperability

while still allowing experiment-specific customisation where needed.

---

## Related concepts

- [Workflow language]({{< ref "workflow-language.md" >}})
- [Workflow compilation pipeline]({{< ref "author-normalise-plan.md" >}})
- [Operations and specs]({{< ref "operations-and-specs.md" >}})
- [Profiles and registries]({{< ref "profiles-and-registries.md" >}})
- [Execution backends]({{< ref "execution-backends.md" >}})

