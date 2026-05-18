---
title: "Getting started"
---

FAST-HEP is distributed as a collection of modular packages.

The recommended entry point is the `fasthep` meta package, which provides curated installation profiles for different use cases.

{{< admonition warning >}}
The FAST-HEP ecosystem is currently undergoing a major rewrite and alpha-stage reorganisation.

Not all packages are available on PyPI yet, and some installation details may change while the ecosystem stabilises.

We will try to keep the main installation entry points stable for their intended use cases.
{{< /admonition >}}

---

## HEP installation

Recommended for most HEP users.

Includes:

- workflow execution
- ROOT and awkward support
- histogramming
- rendering
- diagnostics
- CLI tooling

```bash
pip install "fasthep[hep]"
```

This is expected to become the standard installation profile for typical HEP analyses.

---

## Full installation

Installs the complete FAST-HEP ecosystem and optional tooling.

```bash
pip install "fasthep[full]"
```

This profile is mainly intended for:

- developers
- integration testing
- ecosystem experimentation

---

## Minimal installation

Installs only the core workflow tooling.

```bash
pip install "fasthep[minimal]"
```

This profile is intended for:

- lightweight workflow experimentation
- non-HEP workflow exploration
- backend/runtime development

Additional functionality can then be added incrementally through individual packages and profiles.

---

## Workshop installation

The workshop repository contains:

- tutorials
- runnable examples
- reference analysis structures
- training material

Repository:

- `fasthep-workshop`

The workshop examples are currently expected to evolve alongside the alpha ecosystem.

---

## Development workspace

Contributors working across multiple packages should use:

- `fasthep-dev`

The development workspace provides:

- editable installs
- cross-package testing
- integration tooling
- shared developer workflows

---

## Next steps

For most users, the best starting point is the workshop material and runnable examples.

Recommended path:

1. Explore the [Tutorials]({{< ref "/tutorials/" >}})
2. Run examples from:
   - [`fasthep-workshop`](https://github.com/FAST-HEP/fasthep-workshop)
3. Read:
   - [Workflow language]({{< ref "/concepts/workflow-language.md" >}})
4. Explore package-specific documentation:
   - especially [fasthep-flow docs](https://fasthep-flow.readthedocs.io/en/latest/)
5. Learn how analyses are structured:
   - [Analysis repositories]({{< ref "/concepts/analysis-repositories.md" >}})

Optional deeper reading:

- [Concepts]({{< ref "/concepts/" >}})
- [Packages]({{< ref "/packages/" >}})
