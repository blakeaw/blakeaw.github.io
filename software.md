---
layout: page
title: Software
subtitle: A selection of software contributions.
---

Note that additional software contributions can be found on my GitHub account page, [github.com/blakeaw](https://github.com/blakeaw), as well as the [NTBEL](https://github.com/NTBEL), [LoLab-MSM](https://github.com/LoLab-MSM), and [Borealis-BioModeling](https://github.com/Borealis-BioModeling) GitHub organizations. 

------
------

# Research Software

Software developed as a part of my research at UT Dallas and Vanderbilt University.


## Web apps 

### Cell/ROI csv plotting App

Use this app to load csv files corresponding to individual cells/ROIs from fluorescent images and plot their data, apply size thresholding, and estimate the dF/F values for each trace.  

Try it out for free on Streamlit Cloud: https://blakeaw-roi-plotter-app-h5ke6x.streamlit.app/

* **Source Code Repo**: https://github.com/NTBEL/roi-plotter
  * Or my personal fork: https://github.com/blakeaw/roi-plotter
* **Language**: Python and the Streamlit framework.
* **Developed at**: The University of Texas at Dallas, Lab of Zhenpeng Qin

### Peptides App 

Use this simple app to predict peptide properties from an amino acid sequence via the `peptides.py` package.

Try it out for free on Streamlit Cloud: https://blakeaw-peptide-property-app-app-gkqzgi.streamlit.app/

* **Source Code Repo**: https://github.com/NTBEL/peptide-property-app
  * Or my personal fork: https://github.com/blakeaw/peptide-property-app
* **Language**: Python and the Streamlit framework.
* **Developed at**: The University of Texas at Dallas, Lab of Zhenpeng Qin

------

## Python programs/packages

#### diffusion-fit

This python package defines models to fit the 2D fluorescence intensity distribution in a time lapse series of fluorescence microscope images and extract estimates of a diffusion coeffient using a point-source paradigm.

* **Source Code Repo**: https://github.com/NTBEL/diffusion-fit
* **Language**: Python.
* **Developed at**: The University of Texas at Dallas, Lab of Zhenpeng Qin

------


#### PyBILT

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3426128.svg)](https://doi.org/10.5281/zenodo.3426128)  

PyBILT (Python based lipid BILayer molecular simulation analysis Toolkit) is a Python toolkit developed to analyze molecular simulation trajectories of lipid bilayers systems. The toolkit includes a variety of analyses from various lipid bilayer molecular simulation publications.

[<img width="100" height="100" src="https://raw.githubusercontent.com/LoLab-VU/PyBILT/master/_images/PyBILT_logo.png">](https://github.com/LoLab-VU/PyBILT)

* **Source Code Repo**: https://github.com/LoLab-VU/PyBILT
  * Or my personal fork: https://github.com/blakeaw/PyBILT
* **Language**: Python.
* **Developed at**: Vanderbilt University, Lab of Carlos F. Lopez

------

#### Gleipnir

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3743840.svg)](https://doi.org/10.5281/zenodo.3743840)

Gleipnir is a python toolkit that provides an easy to use interface for Bayesian parameter inference and model selection using Nested Sampling. It has a built-in implementation of the classic Nested Sampling algorithm but also provides a common interface to the Nested Sampling implementations MultiNest, PolyChord, dyPolyChord, DNest4, and Nestle.

[<img width="100" height="100" src="https://raw.githubusercontent.com/LoLab-VU/Gleipnir/master/images/gleipnir_logo_2.png">](https://github.com/LoLab-VU/Gleipnir)

* **Source Code Repo**: https://github.com/LoLab-VU/Gleipnir
  * Or my personal fork: https://github.com/blakeaw/Gleipnir
* **Language**: Python.
* **Developed at**: Vanderbilt University, Lab of Carlos F. Lopez

------

#### BioComparator

BioComparator is a Python utility for comparing biological models encoded with [PySB](http://pysbdev.lolab.xyz/). BioComparator fits candidate models to a common data set and provides users with a set of comparative metrics that allows them to evaluate the models' fit to the data and the trade offs between the fit to data and model size/complexity.

* **Source Code Repo**: https://github.com/LoLab-VU/BioComparator
* **Language**: Python.
* **Developed for**: Contract with Vanderbilt University, Lab of Carlos F. Lopez

------
------

# Personal Projects 

## Web Apps

### Aurora PK/PD

Open Web App for Pharmcological Modeling and Analysis.

Try it out for free on Streamlit Community Cloud: https://aurora-pkpd.streamlit.app/

* **Source Code Repo**: https://github.com/Borealis-BioModeling/aurora-pkpd
* **Language**: Python and the Streamlit framework.

------

### 52 Hike App 

Use this web app to view and analyze data for hikes completed as a part of the 52 Hike Challenge.

Try it out for free on Streamlit Community Cloud: https://52-hike-app.streamlit.app/

* **Source Code Repo**: https://github.com/blakeaw/52-hike-app
* **Language**: Python and the Streamlit framework.

------

### Insurance Pro Rata Calculator 

Use this web app to calculate pro rata insurance rates.

Try it out for free on Streamlit Community Cloud: https://blakeaw-insurance-pro-rata-calculator-streamlit-app-rryoqp.streamlit.app/

* **Source Code Repo**: https://github.com/blakeaw/insurance-pro-rata-calculator
* **Language**: Python and the Streamlit framework.

------

## Python programs/packages

#### pysb-units

[![DOI](https://zenodo.org/badge/809880137.svg)](https://zenodo.org/doi/10.5281/zenodo.12786380)

**pysb-units** is an add-on for the PySB modeling framework that streamlines unit management and helps ensure unit consistency in PySB models.

* **Language:** Python.
* **Source Code Repo:** [Borealis-BioModeling/pysb-units](https://github.com/Borealis-BioModeling/pysb-units)

------

#### pysb-pkpd

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.12775536.svg)](https://doi.org/10.5281/zenodo.12775536)

**pysb-pkpd** is an add-on for the PySB modeling framework that enables you to efficiently program and simulate dynamic PK/PD and QSP models in Python using PySB.

provides domain-specific macros and pre-constructed models for empirical and mechanistic PK/PD modeling.

* **Language:** Python.
* **Source Code Repo:** [github.com/blakeaw/pysb-pkpd](https://github.com/blakeaw/pysb-pkpd)

------

#### GAlibrate

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3903010.svg)](https://doi.org/10.5281/zenodo.3903010)

GAlibrate is a python toolkit that provides an easy to use interface for model calibration/parameter estimation using an implementation of continuous genetic algorithm-based optimization. Performance scaling analysis available in [this Jupyter notebook](https://github.com/blakeaw/galibrate_performance_comparison/blob/master/gao_implementation_scaling_comparison.ipynb).

[<img width="100" height="100" src="https://raw.githubusercontent.com/blakeaw/GAlibrate/master/images/GAlibrate_logo.png">](https://github.com/bblakeaw/GAlibrate)

* **Language:** Python
* **Source Code Repo** [github.com/blakeaw/GAlibrate](https://github.com/blakeaw/GAlibrate) 

------

#### ObjectiveLearner

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3408007.svg)](https://doi.org/10.5281/zenodo.3408007)

**ObjectiveLearner** is a python module that provides functionality to run [Supervised Machine Learning](https://en.wikipedia.org/wiki/Machine_learning#Supervised_learning) (linear regression) and [Sensitivity Analysis](https://en.wikipedia.org/wiki/Sensitivity_analysis#Regression_analysis) on the objective function versus parameter relationship using the thousands (to millions) of (sometimes expensive) objective function evaluations performed during model calibration with packages like [PyDREAM](https://github.com/LoLab-VU/PyDREAM), [simplePSO](https://github.com/LoLab-VU/ParticleSwarmOptimization), [Gleipnir](https://github.com/LoLab-VU/Gleipnir), and [GAlibrate](https://github.com/blakeaw/GAlibrate).

* **Language:** Python
* **Source Code Repo** [github.com/blakeaw/ObjectiveLearner](https://github.com/blakeaw/ObjectiveLearner)

------

## Other

#### Bash Adventure

Bash Adventure is a short and simple text-based RPG style game written in Bash script that you can play in the terminal. Although I've yet to get around to smoothing out some of the rough edges (hard to believe it's been more than half a decade since I wrote the game), it should still provide a few minutes (maybe like 5 to 10 or so) of classic text-based gaming entertainment.

* **Language:** Bash
* **Source Code Repo**: [github.com/blakeaw/BashAdventure](https://github.com/blakeaw/BashAdventure)


