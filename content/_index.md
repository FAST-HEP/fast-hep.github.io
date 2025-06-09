# FAST-HEP

FAST-HEP is a group of High Energy Physics (HEP) researchers who:
- Contribute to existing software tools and/or develop its own [packages]({{< ref "packages" >}}) to close gaps in the High Energy Physics software ecosystem.
- Develop and showcase an “ideal FAST-HEP analysis” and demonstrate its capabilities and performance.
- Provide expertise on how to incorporate modern analysis techniques into analyses (eg. deep machine learning, vectorised array programming, etc), and provides feedback to the developers of such tools.
- Educate FAST-HEP members and colleagues on what modern analysis tools and techniques are available and encourages knowledge sharing through presentations and workshops.

The current set of interacting packages is described in this figure: 

[![FAST-HEP packages](/images/package_overview-colour-description-all_packages.png)](/images/package_overview-colour-description-all_packages.png)

The [packages]({{< ref "packages" >}}) are available on [pypi](https://pypi.org/user/fast-hep/) and their code on [GitHub](https://github.com/fast-hep/). Installation can be achieved through, for example, the `fasthep` meta-package:

```bash
  pip install fasthep[full]
```

## Goals
> **Note:**  "At the end of the day, when you're making something, the hardest part to understand [...] is how data are originated, how they are manipulated, and how they are displayed." - The Primeagen, probably.

FAST-HEP aims to provide tools that help HEP researchers to describe:
- where data come from (fasthep-curator)
- how data are manipulated (fasthep-flow + fasthep-carpenter)
- how data are displayed (fasthep-flow + fasthep-plotter)

## Who is using it
FAST-HEP is currently used by members of: the [CMS Collaboration](https://cms.cern) (including analysis and upgrade work), the [LZ Dark Matter Experiment](https://lz.lbl.gov/), and the [Deep Underground Neutrino Experiment](https://www.dunescience.org/) (DUNE).
Collaborations or individuals that are interested in our tools or need support can reach us at [fast-hep@cern.ch](mailto:fast-hep@cern.ch). We also have a community on gitter: https://gitter.im/FAST-HEP/community.

The tools are written in python and are powered by external libraries and tools: [pandas](https://pandas.pydata.org/), [numpy](https://www.numpy.org/), [matplotlib](https://matplotlib.org/), [scikit-hep](https://scikit-hep.org/) (in particular [uproot](https://github.com/scikit-hep/uproot>)), [Dask](https://www.dask.org/), etc.

## Contributing

## Contents

{{< toc-tree >}}