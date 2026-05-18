---
title: "Operations and specs"
weight: 3
---

Operations are the primary executable units in FAST-HEP workflows.

An operation describes:

- what computation should happen
- which inputs are required
- which outputs are produced
- how the planner should reason about dependencies

Operations are identified declaratively in workflows using an operation kind:

```yaml
op: hep.define
```

The actual implementation is resolved through registries during workflow compilation.

---

## Why operations exist

FAST-HEP separates:

- workflow structure
- execution planning
- runtime implementations

Operations are the bridge between declarative workflow descriptions and executable analysis logic.

This allows workflows to remain:

- composable
- inspectable
- backend-independent
- extensible through registries

---

## Common operation categories

Operations may perform many different tasks.

| Category | Purpose | Example | Typical package |
|---|---|---|---|
| sources | introduce external data streams | `root_tree` | `fasthep-carpenter` |
| transforms | derive or modify event data | `hep.define` | `fasthep-carpenter` |
|  | apply filtering and cutflows | `hep.selection.cutflow` | `fasthep-carpenter` |
|  | aggregate or summarise event data | counters, reductions | `fasthep-carpenter` |
| sinks | persist or aggregate outputs | histogram filling, ROOT/parquet writing | `fasthep-carpenter` |
|   | generate plots and reports | `hep.render.data_mc` | `fasthep-render` |
| observers | inspect workflow state | schema snapshots | `fasthep-curator` |
| hooks | react to runtime lifecycle events | diagnostics, summaries | `fasthep-curator` |

The exact boundary between categories may evolve as the ecosystem matures, but the core separation of responsibilities between packages is intentional.

{{% note %}}
Rendering is currently registered as a sink because it consumes produced artifacts and writes visual outputs. It may become a more explicit operation category as the rendering API stabilises.
{{% /note %}}

---

## Example: derived variables

Example operation:

```yaml
- id: BasicVars
  op: hep.define
  params:
    variables:
      - name: Muon_Pt
        expr: "sqrt(Muon_Px ** 2 + Muon_Py ** 2)"
```

This operation:

- depends on:
  - `Muon_Px`
  - `Muon_Py`

- produces:
  - `Muon_Pt`

FAST-HEP uses this information during dependency inference and planning.

---

## Example: histogramming

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

This operation consumes event-level quantities and produces histogram artifacts.

Histogram operations are commonly followed by rendering operations.

---

## Example: rendering

Rendering is usually attached declaratively to produced artifacts:

```yaml
render:
  style: dimuon_mass
  when: final
```

Rendering operations may produce:

- plots
- tables
- reports
- dashboards
- summaries

The rendering implementation is resolved through registries and styles.

---

## Specifications

Operations are accompanied by specifications ("specs").

Specs describe operation behavior in a planner-friendly and machine-readable form.

Depending on the operation type, specs may describe:

- required inputs
- produced outputs
- stream behavior
- aggregation semantics
- partition behavior
- render expectations
- configuration schemas

Specs help FAST-HEP reason about workflows before runtime execution begins.

---

## Why specs matter

Specs allow planners to understand workflows without executing analysis code.

This enables:

- dependency inference
- validation
- execution planning
- artifact prediction
- schema reasoning
- runtime diagnostics

without requiring immediate data processing.

This separation is one of the key architectural ideas in FAST-HEP.

---

## Planner-visible behavior

Operations often expose planner-visible behavior separately from runtime implementations.

For example:

- runtime implementation
  - performs the actual computation

- spec implementation
  - describes dependencies and outputs

This allows planners to construct execution graphs before runtime execution begins.

---

## Registry integration

Operations are registered through registries.

For example:

```yaml
op: hep.hist
```

may resolve to:

- a spec implementation
- a runtime implementation
- render integrations
- diagnostics hooks

provided by `fasthep_carpenter`.

This resolution occurs during workflow compilation.

For more detail, see:

- [Profiles and registries]({{< ref "profiles-and-registries.md" >}})

---

## Runtime independence

Operations intentionally avoid coupling workflows to specific runtimes.

The same operation definitions may eventually support:

- local execution
- distributed execution
- GPU execution
- batch/grid systems

without changing the workflow description itself.

---

## Historical note

Earlier FAST-HEP prototypes exposed some operation specifications as generated JSON artifacts.

This made planner-visible behavior easier to inspect and debug.

Future FAST-HEP versions may again expose richer spec artifacts and planner diagnostics as part of workflow introspection tooling.

---

## Design philosophy

Operations and specs intentionally separate:

- declarative workflow meaning
- planning semantics
- runtime execution details

This separation helps workflows remain:

- inspectable
- extensible
- portable
- easier to validate
- easier to debug

---

## Related concepts

- [Workflow language]({{< ref "workflow-language.md" >}})
- [Workflow compilation pipeline]({{< ref "author-normalise-plan.md" >}})
- [Profiles and registries]({{< ref "profiles-and-registries.md" >}})
- [Execution backends]({{< ref "execution-backends.md" >}})
- [Analysis repositories]({{< ref "analysis-repositories.md" >}})

