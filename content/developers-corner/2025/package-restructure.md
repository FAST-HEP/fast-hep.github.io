+++
weight = 1
+++

To ensure production workflows remain stable and uninterrupted—while also improving consistency and visibility across the ecosystem—all fast-* packages will undergo a planned restructure and renaming process.

### Renaming and Consolidation

- All `fast-*` packages will be renamed to follow the `fasthep-*` naming convention.
- `scikit-validate` will become `fasthep-validate`.
- Three new packages will be introduced:
  - **`fasthep-cli`**: A unified command-line interface replacing the individual CLIs in existing `fast-*` packages.
  - **`fasthep-workshop`**: A curated set of Jupyter notebooks demonstrating real-world usage in a workshop-style format.
  - **`fasthep`**: A meta-package simplifying installation of the entire FAST-HEP stack — or subsets of it.
  - **`fasthep-toolbench`**: A package containing utility functions that are useful across multiple packages, such as file handling, logging, and other common tasks.

An overview of the planned packages is shown below:
```mermaid
flowchart TB
    
    fasthep("fasthep")
    subgraph Utility["`**Utility**`"]

        cli("fasthep-cli")
        workshop("fasthep-workshop")
        toolbench("fasthep-toolbench")
    end

    subgraph Core["`**Core**`"]

        carpenter("fasthep-carpenter")
        curator("fasthep-curator")
        flow("fasthep-flow")
    end

    subgraph Extensions["`**Extensions**`"]

        plotter("fasthep-plotter")
        validate("fasthep-validate")
        report("fasthep-report")
        stats("fasthep-stats (TBC)")
    end

    style fasthep fill:#9333f8,stroke:#333,stroke-width:2px,color:white,font-size:16pt;

    style Utility fill:#d2e2f2,stroke:#333,stroke-width:2px,color:#4da6ff,font-size:12pt;
    style cli fill:#4da6ff,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style workshop fill:#4da6ff,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style toolbench fill:#4da6ff,stroke:#333,stroke-width:2px,color:white,font-size:16pt;

    style Core fill:#fdf2cf,stroke:#333,stroke-width:2px,color:#f49c2f,font-size:12pt;
    style carpenter fill:#f49c2f,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style curator fill:#f49c2f,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style flow fill:#f49c2f,stroke:#333,stroke-width:2px,color:white,font-size:16pt;

    style Extensions fill:#dbe9d4,stroke:#333,stroke-width:2px,color:#73a557,font-size:12pt;
    style plotter fill:#73a557,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style validate fill:#73a557,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style stats fill:#73a557,stroke:#333,stroke-width:2px,color:white,font-size:16pt;
    style report fill:#73a557,stroke:#333,stroke-width:2px,color:white,font-size:16pt, a.text-decoration: none;

    fasthep --> Utility
    fasthep --> Core
    fasthep --> Extensions

    click fasthep "https://github.com/FAST-HEP/fasthep" _blank
    click cli "https://github.com/FAST-HEP/fasthep-cli" _blank
    click workshop "https://github.com/FAST-HEP/fasthep-workshop" _blank
    click toolbench "https://github.com/FAST-HEP/fasthep-toolbench" _blank
    click carpenter "https://github.com/FAST-HEP/fasthep-carpenter" _blank
    click curator "https://github.com/FAST-HEP/fasthep-curator" _blank
    click flow "https://github.com/FAST-HEP/fasthep-flow" _blank
    click plotter "https://github.com/FAST-HEP/fasthep-plotter" _blank
    click validate "https://github.com/FAST-HEP/fasthep-validate" _blank
    click stats "https://github.com/FAST-HEP/fasthep-stats" _blank
    click report "https://github.com/FAST-HEP/fasthep-report" _blank
```


Not all of these packages exist at the moment, for details check the status below.

### fasthep

| Package | Description | Installation | Status |
| --- | --- |--- |--- |
| **fasthep** | Metapackage for all FAST-HEP packages for easier installation | `pip install fasthep[full\|core\|dev]` | :yellow_circle: Ready for testing |
| **fasthep-cli** |  Unified Command Line Interface for all FAST-HEP packages | ``pip install fasthep-cli`` | :yellow_circle: Ready for testing |
| **fasthep-logging** | Logging package - provides "trace" and "timing" log-levels | ``pip install fasthep-logging`` |:green_circle: Completed |
| **fasthep-gitlab** | Refactor of gitlab components from scikit-validate | `pip install fasthep-gitlab` | :red_circle: None |
| **fasthep-carpenter** | Rewrite of fast-carpenter | `pip install fasthep-carpenter` | :orange_circle: In Development |
| **fasthep-curator** | Clone of fast-curator. | `pip install fasthep-curator` | :yellow_circle: Ready for testing |
| **fasthep-flow** | Clone of fast-flow. | `pip install fasthep-flow` | :orange_circle: In Development |
| **fasthep-plotter** | Rewrite of fast-plotter |`pip install fasthep-plotter` | :red_circle: None |
| **fasthep-toolbench** | Package for cross-package utility functions |`pip install fasthep-toolbench` | :red_circle: None |
| **fasthep-validate** | Rewrite of scikit-validate | `pip install fasthep-validate` | :red_circle: None |
| **fasthep-stats** |Rewrite of fast-datacard?? | `pip install fasthep-stats` | :red_circle: None |

