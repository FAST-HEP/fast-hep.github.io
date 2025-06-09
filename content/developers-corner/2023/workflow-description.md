+++
weight = 2
geekdocHidden = true
+++

[![workflow example](/images/local-workflow.png)](/images/local-workflow.png)

## Requirements for stages
To fit the `carpenter` stages into the proposed workflow they must satisfy some requirements.

| Stage | Requirements |
| ---   | -----------  |
| data import | Translate data config into a data mapping - mapping should be dictionary-like and provide n-D array access |
| data mapping | copy on write; accommodate new data; GPU compatible; aliases &rarr; Index; filtering &rarr; Mask |
| Index | `Callable` (order?); uproot path: `dir1/tree1/var1`; alias: `dir1.tree1.var1`; expression: `dir1__dot__tree1__dot__var1` (or use a simplified version) |
| Mask(expression) |`Callable`, `Mergable`; merge via minary `AND`, `OR`: `(mask1 \| mask2)` &rarr; `mask3`, `(mask1 & mask2)` &rarr; `mask4` |
| Operations(config) | `Callable`, `Mergable`; Types: Define &rarr; new data; Cutflow &rarr; creates Masks + cutflow; Binning &rarr; createst Hists, tables; DataOut &rarr; creates ntuples/CSV/binary format |

### Merging functionality
- `Define` stage: independent new entries &rarr; data.update
- `Cutflow` stage: merging counts across files and datasets
- `Binning` stage: merge bin entries (preserve datasets?)

&rarr; each type of merge needs its own rules

### Multiplexing

For optimization, we need to be able to replicate stages across inputs - if applicable.
Stage multiplexing will replicate a stage definition across inputs (previous stages, data import).
Stages that merge data, will typically have different rules for muliplexing (none, reduce by `N`).




[![Under construction](/images/under_construction.avif)](/images/under_construction.avif)