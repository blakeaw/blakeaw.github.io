---
layout: page
title: Models
subtitle: Biological and other model contributions.
---

## PySB Models

Biochemical systems models encoded using the [PySB](https://pysb.org/) framework: 

### PAR2 Activation and calcium signaling Reaction Model (PARM)

Models of agonist-induced activation of PAR2 and calcium signaling via the phospholipase C and IP3 pathway.
* **Type:** Dynamical differential equation-based (ODE/SDE) model.
* **Language:** Python via [PySB](https://pysb.org/)
* **GitHub Repo:** [NTBEL/PARM](https://github.com/NTBEL/PARM)

------

### Model of Yeast Glycolitic Oscillations (MYGO)

[PySB](https://pysb.org/) implementation of a simple model of yeast glycolytic oscillations adapted from Bier et al (Biophys. J. 78, 1087-1093, 2000; https://doi.org/10.1016/S0006-3495(00)76667-7).

* **Type:** Dynamical differential equation-based (ODE/SDE) model. 
* **Language:** Python via [PySB](https://pysb.org/).
* **GitHub Repo:** [blakeaw/MYGO](https://github.com/blakeaw/MYGO)

------

### Three-component repressive network model

[PySB](https://pysb.org/) implementation of the generic three component repressor model described in Figure 1 of Mogilner et al. (Developmental Cell 11, 279-287, 2006; https://doi.org/10.1016/j.devcel.2006.08.004)

* **Type:** Dynamical differential equation-based (ODE/SDE) model.
* **Language:** Python via [PySB](https://pysb.org/)
* **GitHub Repo:** [blakeaw/three-component-repressive-network-model](https://github.com/blakeaw/three-component-repressive-network-model)


------

### PK/PD models

[PySB](https://pysb.org/) add-on for PK/PD modeling that includes some pre-defined PK `pysb.pkpd.pk_models` and PK/PD (`pysb.pkpd.models`) models. 

* **Type:** Dynamical differential equation-based (ODE) models.
* **Language:** Python via [PySB](https://pysb.org/)
* **GitHub Repo:** [blakeaw/pysb-pkpd](https://github.com/blakeaw/pysb-pkpd)

------
------

## NEURON Models

Reaction-diffusion models encoded using the [NEURON](https://www.neuron.yale.edu/neuron/) framework:

### Extracellular diffusion models

Models of fluorescent dye and neuropeptide release and diffusion in the brain extracellular space.
* **Type:** Dynamical reaction-diffusion (PDE+ODE) models
* **Language:** Python via the [NEURON](https://www.neuron.yale.edu/neuron/) reaction-diffusion module.
* **GitHub Repo:** [NTBEL/extracellular-models](https://github.com/NTBEL/extracellular-models)


------
------

## Models for data fitting

### Pharmacodynamic response models

Library of pharmacodynamic models of concentration and dose-response, inhibitor-response, and receptor-response that can be used for empirical fitting of response data.
* **Type:** Functions for empirical fitting.
* **Language:** Python
* **GitHub Repo:** [NTBEL/pharmacodynamic-response-models](https://github.com/NTBEL/pharmacodynamic-response-models)

------

### Diffusion fitting models

Models for non-linear fitting of 2D fluorescence imaging data and estimation of diffusion coefficients based on point-source and/or quantal release models. 
* **Type:** Classes and functions for empirical fitting. 
* **Language:** Python
* **GitHub Repo:** [NTBEL/diffusion-fit](https://github.com/NTBEL/diffusion-fit)

------


