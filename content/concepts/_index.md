---
title: "Concepts"
weight: 10
---

FAST-HEP separates the scientific description of an analysis from the
implementations and computing infrastructure used to execute it.

An analysis begins with a workflow description. Flow validates and normalises
that description, resolves registered capabilities and dependencies, constructs
a logical graph, and produces an execution plan before runtime execution begins.

The following pages describe the main concepts behind this process.

## Core concepts

### [Workflow language]({{< ref "workflow-language.md" >}})

How analyses are described in `workflow.yaml`, including datasets, sources,
analysis operations, outputs, and rendering.

### [Compilation and execution]({{< ref "compilation-and-execution.md" >}})

How Flow turns a workflow description into a logical graph and execution plan,
and how that plan separates scientific workflow semantics from runtime
execution.

### [Operations and specs]({{< ref "operations-and-specs.md" >}})

How workflow operations expose their parameters, inputs, outputs, dependencies,
and runtime behaviour through stable contracts.

### [Profiles and registries]({{< ref "profiles-and-registries.md" >}})

How capabilities are discovered and selected, and how profiles compose
registries for different analysis and execution contexts.

### [Execution environments]({{< ref "execution-environments.md" >}})

How the same execution plan can be mapped onto different backends, computing
resources, and provisioning strategies.

### [Analysis repositories]({{< ref "analysis-repositories.md" >}})

How complete analyses can be organised into reproducible repositories
containing workflows, configuration, custom code, data references, and
supporting material.
