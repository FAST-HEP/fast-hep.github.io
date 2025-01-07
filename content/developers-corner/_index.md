FAST-HEP has been conceived as a platform to try new ideas.
In some ways we've fallen victim to our successes and production use-cases make it hard to make changes.
Our current and past efforts have this always in mind as we want to generate paths for both production and development.

This section is displaying current development efforts and their goals - an [archive]({{< ref "archive" >}}) of older efforts will be put up.


## Current development

FAST-HEP is currently undergoing a major rewrite of most packages.

The goals of this developments are:
1. support workflows that are currently not supported by packages like coffea (e.g. mult-tree)
1. provide a stable, HEP-agnostic backend for development and production (Dask)
1. provide interfaces for better separation of external dependencies
1. Reduce the overall maintainability burden

In order to keep production workflows undisturbed (and for better "marketing"), packages will be renamed while they undergo a restructure.

## Package restructure

[![FAST-HEP package restructure](/images/package_restructure.png)](/images/package_restructure.png)

For the "Extensions" the plan is to rewrite these packages as stages/plugins to the `carpenter` package.

Not all of these packages exist at the moment, for details check [package restructure]({{< ref "2023/package-restructure" >}}).

## Workflow description

## Encapsulating External Dependencies

## Reducing the Maintainability burden