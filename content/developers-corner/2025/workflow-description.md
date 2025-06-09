+++
weight = 2
+++

The most distinctive feature of the FAST-HEP stack is the ability to describe workflows for data analysis and let the package handle the implementation details. This is done through the `fasthep-flow` package, which provides a unified interface for defining and executing workflows.
However, while successful in many cases, the current workflow description presents several challenges that limit extensibility, maintainability, and long-term sustainability. This section outlines the problems and our planned improvements.

## Data agnostic workflow description
The current workflow description is strictly depends on ROOT trees as input, and while multi-tree analyses are possible, they are not well supported. 
With the separation of the workflow implementation (uproot import as a module), we can now, in theory, support any data input, including databases.
The roadmap for this is to:
- improve the current data model to normalise data access across tasks (e.g. `data["task_name"]`)
- create plugins to add accessor functions for non-standard data formats (aliasing, `getter` functions, etc.) - these can be user-defined
- add data output plugins to allow for writing data in different formats (e.g. ROOT, Parquet, etc.)

## Multi-path workflows
At the moment it is only possible to define a sequence of tasks, where the output of one task is the input to the next task.
This is a limitation for more complex workflows, where multiple paths may be needed to process the data (e.g. many control-regions, systematic variations, or ML workflows).
To address this, we take inspiration from the GitLab CI/CD model, where jobs can be run in parallel if their dependencies are described.
We also want to extend the workflow implementation to allow for in-situ changes to the workflow, such as executing a task on **GPU** or **CPU** depending on execution context or loading part of the workflow from cache if present.
Finally, we want to allow for multiple independent workflows to be defined in the same configuration, which can then be executed independently or in parallel.
This is particularly useful for analyses that have multiple semi-independent branches that share some common tasks.
Examples of this are analyses with multiple control regions, where the control region is used to estimate the background or calculate an ML classifier that is then used in the main workflow (e.g. see [fasthep-flow issue 61](https://github.com/FAST-HEP/fasthep-flow/issues/61)).

## Reproducibility and workflow archiving
Reproducibility is a key requirement for scientific workflows, and the current workflow description does not provide a good way to archive and reproduce workflows.
With the introduction of configuration and workflow versioning, we will be able to archive workflows in a way that allows them to be reproduced at a later date.
This will also include a description of the execution environment for tracking down issues.