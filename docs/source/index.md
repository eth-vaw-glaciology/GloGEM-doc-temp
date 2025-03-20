# The Global Glacier Evolution Model (GloGEM)

The Global Glacier Evolution Model is an IDL package which allows to model the evolution of all 200,000 glaciers on Earth outside the ice sheets. 
The model is forced by monthly/daily temperature and precipitation from 14 GCMs and three emission scenarios. In contrast to previous global-scale glacier models, 
GloGEM includes mass loss due to frontal ablation of marine-terminating glaciers. To get started, please check out this [Introduction](Introduction.md) of the model, 
which includes a [schematic overview](Introduction.md#schematic-overview).

## Cite GloGEM

If you want to refer to GloGEM in your publications or presentations, please refer to:

**Huss, M. and Hock, R.: A new model for global glacier change and sea-level rise. Frontiers in Earth Science, 3, 5, https://www.doi.org/10.3389/feart.2015.00054, 2015**

## Glacier model type

GloGEM is a “glacier centric model”, which means that it runs for each glacier independently of the others. In the case of glacier complexes, 
it relies on the glacier inventory to properly separate the individual glacier entities by the ice divides, ensuring that all ice in a glacier 
basin flows towards a single glacier terminus.

```{note}
This project is under active development.
```

```{toctree}
:maxdepth: 2
:titlesonly:
:caption: Overview

Introduction.md
Input-data.md
Downscale-GCM.md
Potential-solar-radiation.md
Mass-balance-model.md
Debris-evolution-model.md
Calving-model.md
Glacier-retreat-model-(VAS).md
Plotting.md
```

```{toctree}
:maxdepth: 2
:titlesonly:
:caption: Reading & Writing Procedures

Reading-of-input-data.md
Writing-of-the-output.md
```

```{toctree}
:maxdepth: 2
:titlesonly:
:caption: Contribution

Contribution.md
Git-workflow.md
```
