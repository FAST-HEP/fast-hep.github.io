+++
weight = 2
+++


Performing a data analysis, typically involves writing software or scripts to execute a series of steps to process and analyze data. This is often referred to as imperative programming, where you specify how to perform the analysis step by step.
Declarative analysis, on the other hand, focuses on describing what you want to achieve without specifying the exact steps to get there. This approach allows for more flexibility and can lead to more concise and readable code. Furthermore, it separates the logic of the analysis from the implementation details, making it easier to adapt and maintain.

In FAST-HEP, we focus on the separation of “the analysis” from “the processing system”.
We believe the main product of an analysis should be a repository of the description of the analysis, not the code that runs it.
As the vessel for this description, we use YAML, a superset of JSON that is easy to read and write.
This allows us to define the analysis in a way that is independent of the specific tools or libraries used to execute it.
YAML are also well-known in the science community, as they are used in Continues Integration (CI) systems like GitHub Actions, GitLab CI, in configuration files for software (Ansible, Kubernetes, etc.), and in [HEPData](https://www.hepdata.net/) entries.

As an example, consider the following YAML file that describes a simple selection:

```yaml
DiMu_controlRegion:	 
    weights: {nominal: weight}
    selection:
        All:
          - Muon_pt > 30
          - leadJet_pt > 100
          - DiMuon_mass > 60
          - DiMuon_mass < 120
          - Any:
            - nCleanedJet == 1
            - DiJet_mass < 500
            - DiJet_deta < 2
```
This YAML file describes a selection called `DiMu_controlRegion` with a set of weights and a selection criteria.
Typically, such a selection would be part of a loop over events in a dataset, but here it can be concisely described in a declarative way.
Underlying tools can then decide on how to best implement this selection, whether it is in a vectorized way (e.g. [awkward arrays](https://awkward-array.org/)), using a GPU, or forwarded to an external system (e.g. Function as a Service).

Since such a description is independent of the implementation, it is much easier to track changes in the analysis.
Combined with provenance information, it allows for a clear understanding of what was done in the analysis and how it was executed, and, most importantly, it allows for easy reproduction of the analysis.
