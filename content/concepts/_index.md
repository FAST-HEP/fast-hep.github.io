---
title: "Concepts"
weight: 10
---

FAST-HEP separates the description of an analysis from the details of how it is executed.

An analysis begins as a declarative workflow describing **what** should be computed. FAST-HEP validates and normalises that description, resolves the available operations, determines their dependencies, and constructs an execution plan.

The following pages describe the main concepts behind this process.

## Core concepts

### [Workflow language]({{< ref "workflow-language.md" >}})

How analyses are described declaratively using `author.yaml`, including data sources, analysis stages, outputs, and rendering.

### [Compilation and execution]({{< ref "compilation-and-execution.md" >}})

How an analysis description becomes an execution plan, and how that plan separates workflow compilation from runtime orchestration.


### [Operations and specs]({{< ref "operations-and-specs.md" >}})

How individual workflow operations are defined and how their inputs, outputs, and configuration are described.

### [Profiles and registries]({{< ref "profiles-and-registries.md" >}})

How FAST-HEP discovers available capabilities and how profiles provide collections of operations for different use cases.

### [Execution environments]({{< ref "execution-environments.md" >}})

How an execution plan can be run using different computing environments and execution systems.

### [Analysis repositories]({{< ref "analysis-repositories.md" >}})

How complete analyses can be organised into reproducible repositories containing workflow descriptions, configuration, code, and supporting material.
