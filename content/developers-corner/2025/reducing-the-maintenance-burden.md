+++
weight = 4
+++

In the current incarnation of the FAST-HEP stack, we have a number of packages that are not well integrated with each other, and that have a high maintenance burden.
This is due to a number of factors, including:
- Inconsistent coding standards and practices across packages
- Difficulty in tracking and managing dependencies between packages
- strong coupling between packages and external libraries, making it hard to update or replace components
- no clear way to allow for both new features and long-term stability of the stack (e.g. via versioning or deprecation strategies)

As a result, we are not able to focus on innovation and new features as much as we would like, and contributors are often overwhelmed by the complexity of the stack.


## Consistent coding standards and practices
To address these issues, we are working on a number of initiatives to reduce the maintenance burden of the FAST-HEP stack. These include:
 - consistent pre-commit configs and CI workflows across packages
 - introduction of type-hints for clearer code and better static analysis
 - consolidation of documentation and examples across packages (API docs remain package-specific)
 - improved dependency management and versioning strategies (switching to Calendar Versioning for all packages)
 - modular refactoring of key packages to improve integration and usability

The biggest change will the the "reimagining" of everything as a **task** or **plugin** for the `fasthep-flow` package, which will serve as the toolbox for analysis building blocks. E.g. instead of special implementations in `fasthep-plotter`, `fasthep-validate`, and `fasthep-stats`, these will be rewritten as tasks and plugins. 
This will make almost every part of FAST-HEP extensible, versionable, and recognisable across packages.

## Improved integration and usability
Instead of every package having its own command line interface, we will introduce a unified command line interface in the `fasthep-cli` package. This will allow users to run tasks and plugins from any package in a consistent way, and will make it easier to develop and test workflows interactively. There is also a big focus on the API, to ensure packages can be used in Jupyter notebooks and other interactive environments. With the introduction of the `fasthep` meta-package, we also hope to consolidate the installation process and make it easier to track dependencies across packages.

## Decoupling of packages and external libraries
Instead of tying external dependencies directly into mission-critical code, most external libraries will be encapsulated in their own **tasks** or **plugins** with well-defined interfaces.
This will allow us to update or replace these components without affecting the rest of the stack and prevent situations like the uproot 3 &rarr; uproot 4 transition, which caused a lot of issues for users and developers alike. Furthermore, we are also decoupling the workflow description from the execution backends, allowing us to support multiple backends (e.g. Dask, Coffea, Hamilton) without having to change the core logic of the packages.

## Versioning of configuration and modules
Previously, a big blocker to trying out new approaches was the riggidity of the configuration and the lack of picking a certain feature without updating the entire stack.
To address this, we will introduce full configuration versioning, including the **tasks** and **plugins**. This will not only allow us to introduce new features as previews, but also the reproducibility of workflows and the ability to roll back to previous versions if needed. 
