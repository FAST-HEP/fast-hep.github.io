---
title: "Profiles and registries"
weight: 4
---

# Profiles and registries

FAST-HEP workflows are extensible through registries and profiles.

Registries provide implementations and capabilities.
Profiles compose and activate those capabilities for workflows.

This mechanism allows FAST-HEP to remain modular while supporting:

- experiment-specific extensions
- custom analysis packages
- alternative execution behavior
- reusable workflow infrastructure

---

## Registries

Registries are the mechanism FAST-HEP uses to map workflow concepts to concrete implementations.

Registries can contribute or override:

- operations (e.g. transforms, histogramming, selections)
- sources (e.g. ROOT trees, toy generators, custom readers)
- sinks (e.g. writing ROOT, parquet, JSON outputs)
- hooks (e.g. diagnostics, execution extensions, summaries)
- renderers (e.g. plots, tables, reports)
- workflow defaults

This allows workflows to remain declarative while different packages provide implementations.

---

## Why registries exist

The FAST-HEP workflow language intentionally does not hardcode:

- ROOT support
- histogramming implementations
- rendering backends
- experiment-specific transforms
- runtime diagnostics
- custom data formats

Instead, functionality is registered through registries.

This separation allows:

- independent package evolution
- third-party extensions
- experiment-specific customisation
- reusable workflow infrastructure
- lightweight analysis packages

---

## Registry contents

A registry typically maps operation kinds to implementations.

Example concepts include:

- operation specifications
- runtime implementations
- source definitions
- render operations
- hook registrations
- sink implementations

For example, a workflow operation:

```yaml
op: hep.define
```

may be resolved through a registry entry contributed by `fasthep_carpenter`.

The workflow language itself does not need to know where the implementation originates.

For more detail on operations and specifications, see:

- [Operations and specs]({{< ref "operations-and-specs.md" >}})

---

## Registry composition and overrides

Multiple registries may be active simultaneously.

Registries can:

- add new functionality
- extend existing functionality
- override previous registrations

This layering mechanism allows analyses and experiments to customise behavior without modifying FAST-HEP core packages directly.

For example:

- a package may provide a custom histogram operation
- an experiment may override a rendering style
- an analysis may register experiment-specific transforms

The resulting workflow environment is assembled during workflow compilation.

---

## Profiles

Profiles are composable bundles of workflow configuration.

A profile typically activates one or more registries together with workflow defaults and runtime configuration.

Profiles allow analyses to construct custom workflow environments without modifying FAST-HEP core packages.

---

## Using profiles

Profiles are activated in `author.yaml`:

```yaml
use:
  profiles:
    - registry
    - fasthep_carpenter:registry
    - fasthep_curator:registry
    - fasthep_render:registry
```

Each entry references a profile resource.

The general form is:

```text
package_name:profile_name
```

For example:

```yaml
- fasthep_render:registry
```

loads:

```text
profiles/registry.yaml
```

from the installed `fasthep_render` package.

---

## Built-in profiles

The core workflow package provides built-in profiles such as:

```yaml
- registry
```

These typically provide:

- core workflow operations
- base execution functionality
- standard planning behavior

Additional packages extend the ecosystem through their own registries and profiles.

---

## Package-provided profiles

Packages commonly expose a registry profile.

Examples include:

| Package | Typical contents |
|---|---|
| `fasthep_carpenter` | transforms, histogramming, ROOT sources |
| `fasthep_curator` | metadata, provenance, diagnostics |
| `fasthep_render` | plotting and rendering operations |
| `fasthep_workshop` | tutorial/example extensions |

A workflow can combine multiple profiles simultaneously.

---

## Extending FAST-HEP

Analysis packages can provide their own profiles and registries.

Example structure:

```text
src/my_analysis/
├── profiles/
│   └── registry.yaml
├── transforms/
├── hooks/
├── sinks/
└── sources/
```

This allows analyses to contribute:

- custom transforms
- custom renderers
- experiment-specific logic
- local workflow extensions

without modifying FAST-HEP core packages.

---

## Profile resolution

FAST-HEP resolves profiles using Python package resources.

For example:

```yaml
- fasthep_workshop:registry
```

causes FAST-HEP to:

1. import `fasthep_workshop`
2. locate:
   ```text
   profiles/registry.yaml
   ```
3. load the profile contents

This mechanism allows extensions to behave like normal installable Python packages.

---

## Composition over inheritance

Profiles are designed to compose rather than deeply inherit from one another.

A workflow may activate multiple independent profiles:

```yaml
use:
  profiles:
    - fasthep_carpenter:registry
    - fasthep_render:registry
    - my_analysis:registry
```

Each profile contributes functionality to the shared workflow environment.

This keeps the ecosystem modular and extensible.

---

## Registries and planning

Registries are resolved before workflow planning begins.

This is important because planners need to understand:

- available operations
- operation specifications
- dependency inference behavior
- runtime capabilities
- render behavior

before execution plans can be constructed.

---

## Design philosophy

Profiles and registries intentionally separate:

- workflow semantics
- implementation details
- experiment-specific behavior
- runtime extensions

This separation helps FAST-HEP support:

- reusable infrastructure
- experiment-specific tooling
- lightweight analysis packages
- independent package releases
- long-term ecosystem evolution

---

## Related concepts

- [Workflow language]({{< ref "workflow-language.md" >}})
- [Workflow compilation pipeline]({{< ref "author-normalise-plan.md" >}})
- [Operations and specs]({{< ref "operations-and-specs.md" >}})
- [Execution backends]({{< ref "execution-backends.md" >}})
- [Analysis repositories]({{< ref "analysis-repositories.md" >}})
