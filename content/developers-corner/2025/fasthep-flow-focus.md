

# FAST-HEP Flow - 2025 focus
FAST-HEP flow is being created from scratch to provide a more flexible and powerful workflow system. The main goals are:
1. Support workflows that are currently not supported by packages like coffea (e.g. multi-tree)
1. Provide a stable, HEP-agnostic backend for development and production of workflows (Hamilton)
1. Provide config and interpretation interfaces to allow for customisation and extension


## Unsupported workflows
Whenever a new package is created, it is usually created with a specific workflow in mind. 
This workflow is then hard-coded, e.g. experiment-specific, into the package and can't be changed without a major rewrite.
fasthep-flow is being designed to be as flexible as possible to allow for arbitrary workflows to be created and run.
It takes care of the flow of data and the execution of operations, but leaves the specifics of the operations to the user or external packages.
In theory, this should allow adapters to be created for unsupported workflows to make them conform to supported ones. Examples of this are:
- Multi-tree workflows &rarr; map to multiple single-tree workflows (e.g. coffea)
- single-event workflows &rarr; map to array-of-event workflows (e.g. alphatwirl)

How this will end up in practice is still to be determined.

## HEP-agnostic backend
In principle, fasthep-flow can encode a workflow in multiple ways.
The internal definition can be adjusted with extensions and the backend and persitence layer can be swapped out (e.g. prefect &rarr; Hamilton).
This should allow for a more stable and flexible workflow system that can be used in a variety of environments.
The first beta will only allow Hamilton as a backend, but this will be expanded in the future.

## Config and interpretation
The config system is designed to be as flexible as possible.
By default, it is a simple YAML file with gitlab-CI/Ansible-like syntax.
The interpretation system uses OmegaConf and hydra to allow for validation and integration with the command line.
In principle, this should allow for a wide range of customisation and extension options, e.g. custom syntax, custom validation, custom interpretation, etc.
While syntax and validation can be swapped out easily, no concrete plans for interpretation have been made yet.
