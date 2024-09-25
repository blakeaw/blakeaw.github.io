---
layout: post
title: Radioligand Therapy (RLT) Part 1 - An Overview
subtitle: Exploring Precision Cancer Therapies
tags: [pharmacology, drug discovery, radioligand therapy, cancer therapies]
---


![image](https://drive.google.com/thumbnail?id=1qFMQlQ_3nzAwdao9trsHSyMhFbNUpV9J&sz=w600)

Recently, I've been learning about radioligand therapies (RLTs), a type of nuclear medicine used for targeted cancer therapy. With this post, I am compiling and sharing what I've learned as a resource for myself and others interested in this therapeutic technology. This post will be the first in a new series I'm calling <i>Exploring Precision Cancer Therapies</i>. I plan to highlight and discuss various targeted and precision cancer therapy approaches.

This post will be the first of two on RLTs. In it, we'll start with an overview of RLTs, covering what they are, how they work, what RLTs are currently available, and some of their challenges. Then, in the next post, we'll construct and explore a semi-mechanistic pharmacokinetics and receptor occupancy (PKRO) model of an RLT. 

______

## Contents

1. [What is Radioligand Therapy?](#what-is-radioligand-therapy)
2. [Currently Available RLTs](#currentlty-available-rlts)
3. [Some Current Challenges of RLTs](#some-current-challenges-of-rlts)
4. [Closing Thoughts](#closing-thoughts)
5. [Acknowledgements/Disclosures](#acknowledgementsdisclosures)
6. [References](#references)

______

## What is Radioligand Therapy?

Radioligand therapy (RLT) is a type of targeted [nuclear medicine](https://www.hopkinsmedicine.org/health/treatment-tests-and-therapies/nuclear-medicine) that uses radioactive material for precision cancer therapy [[1](https://www.novartis.com/us-en/sites/novartis_us/files/rlt-manufacturing-factsheet.pdf)]. RLTs combine two key components: a targeting ligand and a radioisotope (or radionuclide) [[1](https://www.novartis.com/us-en/sites/novartis_us/files/rlt-manufacturing-factsheet.pdf), [2](https://www.novartis.com/research-and-development/technology-platforms/radioligand-therapy)].

![image](https://drive.google.com/thumbnail?id=1nducfnivswwCntrAWqIxH9HmDLWvl6yQ&sz=w200)

The targeting ligand selectively binds to a particular biomarker, like a receptor or antigen expressed by the cancer cells. When the radioisotope (or radionuclide) undergoes [radioactive decay](https://www.epa.gov/radiation/radioactive-decay), it emits a high-energy [ionizing radiation](https://www.epa.gov/radiation/radiation-basics#ioniandnonioni) that damages nearby cells and tissue. This combination allows RLTs to selectively target the cancer cells expressing the targeted biomarker and release cell-damaging radiation in the local vicinity of the cancer.   

![image](https://drive.google.com/thumbnail?id=1_t6vZ-kIrOtHwIf3GGNw3Hc_KN1aGdlE&sz=w400)

As a precision approach, RLTs can target cancer cells throughout the body with limited damage to healthy cells and tissue, allowing them to be used effectively against even metastatic cancers.

## Currentlty Available RLTs

Currently, there are only a handful of FDA-approved RLTs. They include Zevalin&reg; (In-111 Ibritumomab Tiuxetan and Yt-90 Ibritumomab Tiuxetan), approved in 2002 for relapsed or refractory low-grade, follicular, or transformed B-cell non-Hodgkin's lymphoma [[3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10905220/), [4](https://www.accessdata.fda.gov/drugsatfda_docs/label/2002/ibriide021902LB.pdf), [5](https://www.accessdata.fda.gov/drugsatfda_docs/appletter/2002/ibriide021902L.htm)]. However, it is rarely used now [[3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10905220/)]. Progenics Pharmaceuticals (now [Lantheus](https://www.lantheus.com/)) received FDA approval for Azedra&reg; (I-131 Iobenguane) in 2018 for the treatment of advanced or metastatic pheochromocytoma or
paraganglioma [[3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10905220/), [6](https://www.accessdata.fda.gov/scripts/cder/daf/index.cfm?event=overview.process&ApplNo=209607), [7](https://www.accessdata.fda.gov/drugsatfda_docs/label/2018/209607s000lbl.pdf)]. And, most recently, there are Pluvicto&reg; (Lu-177 vipivotide tetraxetan) developed by [Novartis](https://www.novartis.com/) and approved in 2022 [[8](https://www.novartis.com/news/media-releases/novartis-pluvictotm-approved-fda-first-targeted-radioligand-therapy-treatment-progressive-psma-positive-metastatic-castration-resistant-prostate-cancer), [9](https://www.accessdata.fda.gov/scripts/cder/daf/index.cfm?event=overview.process&ApplNo=215833), [10](https://www.linkedin.com/posts/chrisdesavi_medicine-pharmaceuticals-healthcare-activity-6984569645236162560-Ezz1?utm_source=share&utm_medium=member_desktop)] and Lutathera&reg; (Lu-177 Dotatate) aquired by Novartis and FDA approved earlier this year (2024) [[11](https://www.accessdata.fda.gov/scripts/cder/daf/index.cfm?event=overview.process&ApplNo=208700), [12](https://www.novartis.com/news/media-releases/novartis-radioligand-therapy-lutathera-fda-approved-first-medicine-specifically-pediatric-patients-gastroenteropancreatic-neuroendocrine-tumors), [13](https://www.novartis.com/news/media-releases/novartis-announces-planned-acquisition-advanced-accelerator-applications-strengthen-oncology-portfolio)], for treatment of metastatic prostate-specific membrane antigen–positive metastatic castration-resistant prostate cancer (PSMA+ mCRPC) and pediatric somatostatin receptor-positive gastroenteropancreatic neuroendocrine tumors (SSTR+ GEP-NETs), respectively.

Although the number of FDA-approved RLTs is currently limited, this treatment technology is still being developed. For example, [The University of Texas MD Anderson Cancer Center](https://www.mdanderson.org/) announced a strategic collaboration earlier this year to co-develop a new radioligand [[14](https://www.mdanderson.org/newsroom/md-anderson-c-biomex-co-develop-radioligand-therapy.h00-159695178.html)], CBT-001, that targets the CA9 (carbonic anhydrase 9) cancer biomarker overexpressed in various cancers. 

## Some Current Challenges of RLTs

RLTs rely on radioisotopes with a relatively short [half-life](https://en.wikipedia.org/wiki/Half-life) (time it takes for 50% of the radisotope to decay), typically on the order of a week or less.

| Radioisotope | Half-life |
| ------------ | --------- |
| Indium-111   | 2.81 days [[4](https://www.accessdata.fda.gov/drugsatfda_docs/label/2002/ibriide021902LB.pdf)] |
| Yttrium-90   |  2.67 days [[4](https://www.accessdata.fda.gov/drugsatfda_docs/label/2002/ibriide021902LB.pdf)]         |
| Iodine-131   |  8.201 days [[7](https://www.accessdata.fda.gov/drugsatfda_docs/label/2018/209607s000lbl.pdf)] |
| Lutetium-177 |  6.647 days [[15](https://www.accessdata.fda.gov/drugsatfda_docs/label/2022/215833s000lbl.pdf)] |
| | |

A short half-life helps limit the time it takes for radioactivity to abate in a patient after RLT administration. However, this presents a unique logistical challenge in that the RLT must be produced, shipped, and administered to patients within days to avoid significant reductions in the therapeutic strength of the drug [[16](https://www.cnbc.com/2023/02/11/radioligand-cancer-therapy-forces-manufacturers-to-race-the-clock.html)]. 

Another major challenge of RLTs is their cost [[16](https://www.cnbc.com/2023/02/11/radioligand-cancer-therapy-forces-manufacturers-to-race-the-clock.html)]. For example, Novartis' Pluvicto&reg; RLT has a list price in the U.S. of around $42,500 per unit [[16](https://www.cnbc.com/2023/02/11/radioligand-cancer-therapy-forces-manufacturers-to-race-the-clock.html)], with a total per patient treatment cost of approximately $122,489 as estimated by the Canadian Agency for Drugs and Technologies in Health [[17](https://www.ncbi.nlm.nih.gov/books/NBK596311/)]. The average unit price in U.S. may be even higher at around $47,939 [[18](https://www.drugs.com/price-guide/pluvicto)]. Initial biomarker testing  also adds significantly to the overall costs associated with the treatment [[17](https://www.ncbi.nlm.nih.gov/books/NBK596311/)].       

Another potential challenge is patient and clinician access to RLTs, as many treatment centers may not have the infrastructure to deliver the therapy [[3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10905220/), [19](https://www.linkedin.com/posts/novartis_preparing-hcs-for-rlt-activity-7242502774683279360-Y-mL?utm_source=share&utm_medium=member_desktop)]. The limited number of active RLT manufacturing facilities may also make access more difficult due to the logistical challenges of delivering RLTs' within their short time window of viability .

It's possible that as companies like Novartis continue to invest in RLT and expand their manufacturing and supply capabilities [[20](https://www.novartis.com/us-en/news/media-releases/novartis-begins-construction-two-new-radioligand-therapy-facilities-us-expanding-its-world-class-rlt-manufacturing-and-supply-network), [21](https://www.novartis.com/research-and-development/technology-platforms/radioligand-therapy/supporting-healthcare-system-readiness-rlt?utm_source=linkedin&utm_medium=social&utm_campaign=EsmoEanm2024&utm_content=video-global-post2)], these challenges may become less significant over time.  


## Closing Thoughts

In this post, we covered some basics of Radioligand Therapy (RLT), including what they are, how they work, what RLTs are currently available, and some associated challenges. RLTs are an exciting avenue of precision cancer therapy, combining biomarker targeting with radioactivity to apply cytotoxic radiation to cancer cells selectively. Although the number of FDA-approved RLTs is currently limited, and there are still some significant challenges associated with this treatment approach, RLTs remain an active area of development in the continued expansion of our arsenal of cancer therapeutics.

Thanks for reading, and have a nice day! Feel free to message me if you have any questions or want to discuss. Be sure to watch for Part 2 on RLTs, which will come soon and include the construction and exploration of a semi-mechanistic pharmacokinetics and receptor occupancy (PKRO) model of an RLT. I'm also open to feedback and suggestions for future posts, so if there is something you think I should cover, let me know! I'm considering a Part 3 on RLTs where I'd discuss therapeutic outcomes of current FDA-approved RLTs, so drop me a line if that interests you. You can reach me by [email](mailto:blakeaw1102@gmail.com) or feel free to hit me up on [LinkedIn](https://www.linkedin.com/in/blakewilson3/).

Lastly, please share this post with anyone else who might be interested. It is a massive help to me, as it increases the blog's visibility and can help more people discover my work. Also, be sure to come back for future posts.

Until next time -- Blake

------

## Acknowledgements/Disclosures

Grammarly was used for proofreading and editing. ChatGPT was used to generate and initial outline of this post.  

Schematics were created using [Inkscape](https://inkscape.org/), with GIMP [GIMP](https://www.gimp.org/) for additional image editing.
 
------

## References

1. <div class="csl-entry"> <i>Manufacturing Fact Sheet: An Overview of Radioligand Therapies and How They Are Made</i>. (n.d.). Retrieved September 19, 2024, from https://www.novartis.com/us-en/sites/novartis_us/files/rlt-manufacturing-factsheet.pdf</div>

2. <div class="csl-entry">Radioligand Therapy | Novartis. (n.d.). Retrieved September 19, 2024, from https://www.novartis.com/research-and-development/technology-platforms/radioligand-therapy</div>

3. <div class="csl-entry">Mittra, E. S., Wong, R. K. S., Winters, C., Brown, A., Murley, S., & Kennecke, H. (2024). Establishing a robust radioligand therapy program: A practical approach for North American centers. Cancer Medicine, 13(3). https://doi.org/10.1002/CAM4.6780 | https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10905220/</div>

4. <div class="csl-entry">FDA Label for Ibritumomab Tiuxetan (Zevalin&trade;). (2001). https://www.accessdata.fda.gov/drugsatfda_docs/label/2002/ibriide021902LB.pdf</div>

5. <div class="csl-entry">FDA Product Approval Letter for Ibritumomab Tiuxetan (Zevalin&trade;). (2001). https://www.accessdata.fda.gov/drugsatfda_docs/appletter/2002/ibriide021902L.htm</div>

6. <div class="csl-entry">Drugs@FDA: FDA-Approved Drugs - Azedra. (n.d.). Retrieved September 20, 2024, from https://www.accessdata.fda.gov/scripts/cder/daf/index.cfm?event=overview.process&ApplNo=209607</div>

7. <div class="csl-entry">FDA Label for Azedra&reg; (iobenguane I 131). (2018). https://www.accessdata.fda.gov/drugsatfda_docs/label/2018/209607s000lbl.pdf</div>

8. <div class="csl-entry">Novartis Pluvicto&trade; approved by FDA as first targeted radioligand therapy for treatment of progressive, PSMA positive metastatic castration-resistant prostate cancer | Novartis. (2022, March 23). https://www.novartis.com/news/media-releases/novartis-pluvictotm-approved-fda-first-targeted-radioligand-therapy-treatment-progressive-psma-positive-metastatic-castration-resistant-prostate-cancer</div>

9. <div class="csl-entry">Drugs@FDA: FDA-Approved Drugs - Pluvicto. (n.d.). Retrieved September 20, 2024, from https://www.accessdata.fda.gov/scripts/cder/daf/index.cfm?event=overview.process&ApplNo=215833</div>

10. <div class="csl-entry">de Savi, C. (n.d.). Pluvicto for Prostate Cancer - LinkedIn Post. Retrieved September 19, 2024, from https://www.linkedin.com/posts/chrisdesavi_medicine-pharmaceuticals-healthcare-activity-6984569645236162560-Ezz1/</div>

11. <div class="csl-entry">Drugs@FDA: FDA-Approved Drugs - Lutathera. (n.d.). Retrieved September 20, 2024, from https://www.accessdata.fda.gov/scripts/cder/daf/index.cfm?event=overview.process&ApplNo=208700</div>

12. <div class="csl-entry">Novartis radioligand therapy Lutathera&reg; FDA approved as first medicine specifically for pediatric patients with gastroenteropancreatic neuroendocrine tumors | Novartis. (2024, April 23). https://www.novartis.com/news/media-releases/novartis-radioligand-therapy-lutathera-fda-approved-first-medicine-specifically-pediatric-patients-gastroenteropancreatic-neuroendocrine-tumors</div>

13. <div class="csl-entry">Novartis announces the planned acquisition of Advanced Accelerator Applications to strengthen oncology portfolio | Novartis. (2017, October 30). https://www.novartis.com/news/media-releases/novartis-announces-planned-acquisition-advanced-accelerator-applications-strengthen-oncology-portfolio</div>

14. <div class="csl-entry">MD Anderson and C-Biomex to co-develop radioligand therapy | MD Anderson Cancer Center. (2024, February 7). https://www.mdanderson.org/newsroom/md-anderson-c-biomex-co-develop-radioligand-therapy.h00-159695178.html</div>

15. <div class="csl-entry">FDA Label for PLUVICTO&trade; (lutetium Lu 177 vipivotide tetraxetan). (2022). https://www.accessdata.fda.gov/drugsatfda_docs/label/2022/215833s000lbl.pdf</div>

16. <div class="csl-entry">Capoot, A. (2023, February 11). Radioligand cancer therapy forces manufacturers to race the clock. CNBC. https://www.cnbc.com/2023/02/11/radioligand-cancer-therapy-forces-manufacturers-to-race-the-clock.html</div>

17. <div class="csl-entry">Canadian Agency for Drugs and Technologies in Health. (2023, March). CADTH Reimbursement Recommendation: Lutetium (177Lu) Vipivotide Tetraxetan (Pluvicto) - NCBI Bookshelf. https://www.ncbi.nlm.nih.gov/books/NBK596311/</div>

18. <div class="csl-entry">Pluvicto Prices, Coupons, Copay Cards & Patient Assistance - Drugs.com. (n.d.). Retrieved September 19, 2024, from https://www.drugs.com/price-guide/pluvicto</div>

19. <div class="csl-entry">Novartis. (2024, September 20). Bringing radioligand therapy to patients hinges on the readiness of our healthcare systems - LinkedIn Post. https://www.linkedin.com/posts/novartis_preparing-hcs-for-rlt-activity-7242502774683279360-Y-mL/</div>

20. <div class="csl-entry">Novartis begins construction of two new radioligand therapy facilities in the US, expanding its world-class RLT manufacturing and supply network | Novartis United States of America. (2024, September 4). https://www.novartis.com/us-en/news/media-releases/novartis-begins-construction-two-new-radioligand-therapy-facilities-us-expanding-its-world-class-rlt-manufacturing-and-supply-network</div>

21. <div class="csl-entry">Supporting healthcare system readiness for RLT | Novartis. (n.d.). Retrieved September 21, 2024, from https://www.novartis.com/research-and-development/technology-platforms/radioligand-therapy/supporting-healthcare-system-readiness-rlt</div>

------
------

Like this content? You can follow this blog and get updated about new posts via my blog's RSS/Atom Feed.

------
------

If you are so inclined, you can also be a financial supporter of my open-source work through Ko-fi:

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/J3J4ZUCVU)

------
------

**Other related posts you might like:**

[Modeling Drug Dynamics using Programmatic PK/PD Models in Python](https://blakeaw.github.io/2023-10-23-pysb-pkpd/)

[Introducing Aurora PK/PD! An Open Web App for Pharmacological Modeling and Analysis](https://blakeaw.github.io/2024-07-29-aurora-pkpd-announcement/)

------
