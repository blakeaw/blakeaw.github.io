---
layout: post
title: Introducing QSPy!
subtitle: An Open-Source framework for rule-based programmatic QSP modeling built on PySB 
tags: [software, QSP, Python, pharmacology, PK/PD, PySB]
---

![image](https://drive.google.com/thumbnail?id=1vGDw3dRvl8zzTHsQuDJCoa06koOo4oo8&sz=h600)

<div class="alert alert-info">
<strong>TL;DR:</strong> I recenlty released the QSPy software for QSP modeling if you want to give it a try: https://borealis-biomodeling.github.io/qspy/
</div>

Hi! Over the last couple of years I've been working on adding functionality to the [PySB]() modeling framework targeted towards PK/PD modeling, which includes the [pysb-pkpd](https://blakeaw.github.io/pysb-pkpd/) and [pysb-units](https://github.com/Borealis-BioModeling/pysb-units) add-ons.  I fairly recently started working to integrate my efforts into developing a new PySB-based package focused on quantitative systems pharmacology (QSP). I'm happy to announce that the development version of the package (i.e., version 0.y.z) is out now:   


**QSPy: Quantitative Systems Pharmacology in Python**

![image](https://drive.google.com/thumbnail?id=1UFHb5S1SP2zR38nlHAQ4Rn2b0UWvDBuc&sz=h100)

Here are the reasons I think you should try QSPy, followed by some of it's key features:

#### Why `QSPy`?

- **Programmatic Modeling** – Enables automated workflows, reproducibility (_e.g., version control and automated testing_), customization, and creation of reusable functions for pharmacological and biochemical processes.
- **Built-in Support for Mechanistic Modeling** – QSPy is built on [PySB](https://pysb.org/)'s mechanistic modeling framework, allowing you to incorporate biochemical mechanisms and build customized mechanistic PK/PD and QSP/QST models.
- **Rule-Based Approach** – Encode complex pharmacological and biochemical processes using intuitive [rule-based modeling](https://en.wikipedia.org/wiki/Rule-based_modeling). No need to enumerate all reactions/molecular species or manually encode the corresponding network of differential equations.
- **Python-Based** – Seamlessly integrates with Python’s scientific computing ecosystem, supporting advanced simulations, data analysis, and visualization.
- **Arbitrary Number of Compartments** – Specify any number of compartments to build custom multi-compartment models, including complex drug distribution and physiologically-based pharmacokinetic (PBPK) models.
- **Enhanced reproducibility and reporting** - With built in tools like the `ModelChecker` and `ModelMetadataTracker`, you can automatically catch potential issues with model structure or components early while also tracking relevant metadata for downstream reproduction and reporting.   
- **Open-Source** - QSPy is free and open-source, meaning it is freely available and fully customizable.

#### Key Features

- **Contextualized model definition** - QSPy introduces a block-based extension of the PySB domain-specific language (DSL) that organizes model components (monomers, parameters, rules, etc.) into named contexts, more closely mimicking the feel of traditional, block-based, DSLs like [BioNetGen](https://bionetgen.org/), [rxode2](https://nlmixr2.github.io/rxode2/), and [mrgsolve](https://mrgsolve.org/). This structure streamlines model definition and improves readability, while remaining fully interoperable with standard class-based definitions and preserving the full flexibility of PySB’s Python-embedded framework.

| PySB  components | QSPy contexts |
| ---- | ------------- |
| <script src="https://gist.github.com/blakeaw/4c57d06538570701811c3556d72741ae.js"></script> | <script src="https://gist.github.com/blakeaw/255e5a3a6358985b452f336f51107304.js"></script> |

- **Native support for units** - In QSPy, models and their parameters can be assigned physical units (e.g., `mg`, `nM`, `hr⁻¹`, `L/min`), enabling automatic conversions, dimensional analysis, and consistency checking.

- **Initial model validation tools** - QSPy provides a `ModelChecker` utility that automatically identifies unused components, zero-valued parameters, missing initial conditions, and overdefined reactions. Warnings are surfaced in real time during model import, with structured logs exported for reproducibility and review.

- **Metadata tracking** - QSPy includes a `ModelMetadataTracker` object that attaches key information, such as author, model version, Python environment, and package versions, directly to the model. This metadata can be exported to a `.toml` file that's both human- and machine-readable, making it easy to track provenance and support downstream reporting or validation workflows.

- **Built-in logging** - Model construction steps, metadata, and redacted provenance are logged to `.qspy/` folders, giving you a reproducible and inspectable trail for every model version.

- **Functional monomer tagging** - QSPy introduces structured tags for classifying monomer components by biological role or modeling intent: e.g., `PROTEIN.RECEPTOR` and `DRUG.INHIBITOR`. These tags add additional expressiveness to model species and enable an additional way to filter monomer components for searches and analyses.

```python
# Using @ operator
Monomer('Drug', ['b']) @ DRUG.INHIBITOR

# Using @= operator
Monomer('Target', ['b'])
Target @= PROTEIN.RECEPTOR

# Inside monomers context
with monomers():
    Decoy = (['b'], None, PROTEIN.RECEPTOR)
```


### Source Code

The QSPy source code is hosted on GitHub [github.com/Borealis-BioModeling/qspy](https://github.com/Borealis-BioModeling/qspy) and is licensed under a permissive BSD 2-Clause license (often considered one of the business-friendly open-source licenses).  


### Wrapping Up

Ultimately, I'm trying to promote a Python-based programmatic approach to rule-based PK/PD and QSP modeling as a serious alternative to other available modeling frameworks, most of which are commercial software or R-based packages (and not rule-based). I'm hopeful that these tools will be useful and provide more tools reproducible PK/PD and QSP modeling. 

Well, that's all I have to say for now. Thanks for reading, and have a nice day! Feel free to shoot me a line if this is a tool you're interested in utilizing for your research or work, or if you have questions. I'm also open to feedback and suggestions! You can reach me by [email](mailto:blakeaw1102@gmail.com) or feel free to hit me up on [LinkedIn](https://www.linkedin.com/in/blakewilson3/).


Lastly, please share this post with anyone else who might be interested. It is a massive help to me, as it increases the blog's visibility and can help more people discover my work. Also, be sure to come back for future posts.


Until next time -- Blake

------

Like this content? You can follow this blog and get updated about new posts via my blog's RSS/Atom Feed.

------

If you are so inclined, you can also be a financial supporter of my open-source work through Ko-fi:


 [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/J3J4ZUCVU)

------

**Other related post you might like:**

[Modeling Drug Dynamics using Programmatic PK/PD Models in Python](https://blakeaw.github.io/2023-10-23-pysb-pkpd/)

[Introducing Aurora PK/PD!](https://blakeaw.github.io/2024-07-29-aurora-pkpd-announcement/)
