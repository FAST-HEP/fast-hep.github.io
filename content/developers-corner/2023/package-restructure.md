+++
weight = 1
+++

[![FAST-HEP package restructure](/images/package_restructure.png)](/images/package_restructure.png)

For the "Extensions" the plan is to rewrite these packages as stages/plugins to the `carpenter` package.

Not all of these packages exist at the moment, for details check the status below.

### fasthep

| Package | Description | Installation | Status |
| --- | --- |--- |--- |
| **fasthep** | Metapackage for all FAST-HEP packages for easier installation | `pip install fasthep[full\|core\|dev]` | :yellow_circle: Ready for testing |
| **fasthep-cli** |  Unified Command Line Interface for all FAST-HEP packages | ``pip install fasthep-cli`` | :yellow_circle: Ready for testing |
| **fasthep-logging** | Logging package - provides "trace" and "timing" log-levels | ``pip install fasthep-logging`` |:green_circle: Completed |
| **fasthep-gitlab** | Refactor of gitlab components from scikit-validate | `pip install fasthep-gitlab` | :red_circle: None |
| **fasthep-carpenter** | Rewrite of fast-carpenter | `pip install fasthep-carpenter` | :orange_circle: In Development |
| **fasthep-curator** | Clone of fast-curator. | `pip install fasthep-curator` | :red_circle: None |
| **fasthep-flow** | Clone of fast-flow. | `pip install fasthep-flow` | :red_circle: None |
| **fasthep-plotter** | Rewrite of fast-plotter |`pip install fasthep-plotter` | :red_circle: None |
| **fasthep-validate** | Rewrite of scikit-validate | `pip install fasthep-validate` | :red_circle: None |
| **fasthep-stats** |Rewrite of fast-datacard?? | `pip install fasthep-stats` | :red_circle: None |