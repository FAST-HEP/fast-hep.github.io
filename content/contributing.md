---
title: Contributing to FAST-HEP
---


Thank you for your interest in contributing to FAST-HEP.

FAST-HEP is a modular ecosystem for declarative High Energy Physics workflows, analysis tooling, rendering, and metadata management.

We welcome contributions of all sizes, including:

* bug reports
* feature requests
* typo fixes
* documentation improvements
* tests
* workflow examples
* infrastructure improvements
* small refactors
* larger architectural work
* AI-assisted contributions

Workflow examples should:
- use public/open data where possible
- or use CERN EOS/CERNBox-accessible data intended for redistribution
- avoid committing large binaries directly into repositories
- include provenance and source information
- be reproducible where practical

Additional guidance and workshop-specific conventions are documented in `fasthep-workshop`.

## Development model

FAST-HEP is developed as a collection of focused packages:

- `fasthep-flow`
  - workflow compilation, planning, and orchestration

- `fasthep-carpenter`
  - HEP analysis transforms and histogramming

- `fasthep-curator`
  - dataset inspection, validation, and metadata generation

- `fasthep-render`
  - plotting, reports, and rendering utilities

- `fasthep-cli`
  - unified command-line interface

- `fasthep-toolbench`
  - shared utilities and user-facing helpers

- `fasthep-workshop`
  - examples, tutorials, and training material

The ecosystem integration workspace is:

* `fasthep-dev`

## Development workflow

Typical workflow:

1. Open an issue or discussion
2. Create a branch
3. Implement changes
4. Run local checks
5. Open a pull request
6. Review and iterate

For substantial refactors or architectural changes, please discuss the proposal before investing significant implementation effort.

Where practical, prefer smaller incremental pull requests over large monolithic changes.

## Development environment

The recommended development environment is based on:

* `pixi`
* Python ≥ 3.11

Most repositories support:

```bash
pixi install
pixi run ci
```

The `fasthep-dev` workspace provides:

* cross-package integration
* shared tooling
* ecosystem-level testing
* coordinated release validation

Contributors working across multiple FAST-HEP packages are encouraged to use `fasthep-dev`.

## Coding standards

FAST-HEP aims to prioritize:

* readability
* maintainability
* modularity
* explicit interfaces
* strong typing
* testability

General guidelines:

* use descriptive names
* keep functions focused
* prefer composition over deeply coupled logic
* avoid unnecessary abstractions
* prefer explicit behavior over implicit magic
* add tests for new functionality
* keep workflow orchestration separate from domain-specific logic

We use:

* `ruff`
* `mypy`
* `pytest`
* `pre-commit`
* GitHub Actions CI

Most repositories support:

```bash
pixi run lint
pixi run typecheck
pixi run test
pixi run ci
```

## Documentation

Documentation improvements are highly valued.

Useful contributions include:

* examples
* tutorials
* API documentation
* workflow diagrams
* migration guides
* debugging notes

Where possible:

* keep examples minimal
* prefer realistic workflows
* document design intent, not only implementation details
* avoid committing generated caches, build outputs, or large derived artifacts unless explicitly required for tests or documentation

## AI-assisted contributions

AI-assisted contributions are welcome.

However, contributors remain responsible for the code they submit.

Contributors should:

* understand the generated code
* review generated changes carefully
* validate correctness locally
* ensure tests pass
* ensure the implementation matches FAST-HEP design principles

Pull requests containing substantial AI-generated or AI-assisted changes should include:

* a short summary of what was generated
* a summary of what was manually reviewed or modified
* explanations for important design decisions
* notes about limitations, assumptions, or areas requiring follow-up review

This additional context helps maintain:

* review quality
* long-term maintainability
* architectural consistency
* contributor trust

Reviewers may reject contributions that:

* cannot be reasonably explained
* introduce unclear abstractions
* add unnecessary complexity
* appear to contain unreviewed generated code
* conflict with ecosystem design principles

AI tools are intended to assist contributors, not replace engineering understanding or reviewer accountability.

## Testing expectations

New functionality should generally include:

* unit tests
* integration tests where appropriate
* regression tests for bug fixes

Tests should:

* be deterministic
* avoid unnecessary external dependencies
* avoid hidden global state
* prefer realistic fixtures over excessive mocking where practical

## Design philosophy

FAST-HEP intentionally separates:

* workflow orchestration
* analysis logic
* rendering
* metadata management
* CLI concerns

This modularity helps:

* ecosystem stability
* independent package evolution
* contributor onboarding
* experiment-specific extension
* future non-HEP reuse of workflow tooling

Contributors are encouraged to preserve these boundaries where possible.

## Releases

Packages are released independently.

The `fasthep` meta package is used to provide:

* verified compatibility bundles
* coordinated ecosystem releases
* curated installation profiles

The `fasthep-dev` workspace is intended to support future ecosystem-level release orchestration and integration validation.

## Communication

Project discussions and questions:

* GitHub Discussions:
  [https://github.com/FAST-HEP/fasthep/discussions](https://github.com/FAST-HEP/fasthep/discussions)

Bug reports and feature requests:

* GitHub Issues:
  [https://github.com/FAST-HEP/fasthep/issues](https://github.com/FAST-HEP/fasthep/issues)

## License

By contributing to FAST-HEP, you agree that your contributions will be licensed under the BSD-3-Clause license used by the project.
