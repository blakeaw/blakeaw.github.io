---
layout: post
title: Introducing Aurora PK/PD!
subtitle: An Open Web App for Pharmacological Modeling and Analysis
tags: [software, Web App, Python, pharmacology, PK/PD, PySB]
---

<div class="alert alert-info">
<strong>TL;DR:</strong> The first release of my new free and open-source Python web app, Aurora PK/PD, is out and running in the Streamlit Community Cloud if you want to give it a try: https://aurora-pkpd.streamlit.app/
</div>

Hi! After a couple of months of stewing on the idea of building a web app for PK/PD modeling, followed by a concerted effort over the last few weeks, I'm happy to announce that I've released the first version of a free and open-source Python web app:


**Aurora PK/PD: Open Web App for Pharmacological Modeling and Analysis**

<img src="https://github.com/Borealis-BioModeling/aurora-pkpd/blob/main/assets/aurora-pkpd-logo-2.png?raw=true" alt="Aurora PK/PD Logo Image" width="200"/>

Last year, I developed the pysb-pkpd package ([here's the blog post](https://blakeaw.github.io/2023-10-23-pysb-pkpd/)), an add-on for the PySB modeling framework, to be a tool that facilitated programming and simulating dynamic PK/PD and QSP models in Python using PySB. The Aurora PK/PD app is a follow-up to that effort meant to help lower the barrier for potential PK/PD modelers who maybe aren't proficient in Python programming, or don't want to spend so much time learning the PySB framework, to get going with PK/PD modeling. Also, the web app approach helps make the tool much more accessible for a broader range of users, as there are no software downloads or installations required to use it. Regardless, the app still utilizes the programmatic approach via PySB under the hood for compartmental PK/PD modeling and ultimately provides a low-code interface for PK/PD modeling with PySB. In this way, advanced users who are comfortable working in Python can use the web app as a starting point for building more sophisticated programmatic models (such as QSP models) or export their models into other programmatic workflows.  Beyond compartmental PK/PD modeling, I ended up expanding the app to include PD analysis with tools to fit data to Exposure-Response models, and I have plans to add additional functionality such as Non-compartmental analysis (NCA) in the coming months.  

Ultimately, I'm hoping that Aurora PK/PD will be helpful for academic researchers and small teams/startups in the drug discovery space that need to utilize pharmacokinetics and pharmacodynamics (PK/PD) models and analysis to advance their drug discovery efforts.


### App Framework

I developed the Aurora PK/PD app using the [Streamlit](https://streamlit.io/) framework, which I've enjoyed working with over the past few years. Even though this one ultimately turned into a much larger multi-page app, and is definitely the most sophisticated Streamlit app I've developed, Streamlit makes it pretty easy to go from Python code to web app without needing full-stack web developer skills. And, it's great that you can deploy your open-source apps, at least as an MVP (minimum viable product), for free on the [Streamlit Community Cloud](https://streamlit.io/cloud). If you're interested in building and deploying Python web/data apps, I highly recommend it.


### Source Code

The Aurora PK/PD source code is hosted on GitHub [Borealis-BioModeling/aurora-pkpd](https://github.com/Borealis-BioModeling/aurora-pkpd) and is licensed under a permissive BSD 2-Clause license (often considered one of the business-friendly open-source licenses).  


### Wrapping Up

Well, that's all I have to say for now. Thanks for reading, and have a nice day! Feel free to shoot me a line if this is a tool you're interested in utilizing for your research or if you have questions. I'm also open to feedback and suggestions! You can reach me by [email](mailto:blakeaw1102@gmail.com) or feel free to hit me up on [LinkedIn](https://www.linkedin.com/in/blakewilson3/).


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

[Python+Numba vs. Julia](https://blakeaw.github.io/2019-09-20-numba-vs-julia/)
