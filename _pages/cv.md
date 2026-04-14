---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* **Ph.D in Computational Systems Biology**, Monash University, 2022 – 2026 (expected)
* **Bachelor of Biomedical Science and Science (Computer Science Major)**, Monash University, 2017 – 2022

Experience
======
* **PhD Candidature**, Monash University, June 2022 – Current
  * Developed ODE-based mathematical models and feature engineering pipelines for cancer drug response prediction
  * Optimised machine learning algorithms and numerical integration on HPC (SLURM/S3) for large-scale training

* **Laboratory Assistant**, Box Hill Hospital, Eastern Health, Sep 2020 – June 2022
  * Processed COVID-19 tests using Evolution pathology software and electronic medical records

Projects
======
* **Synthetic** | Project Author, Dec 2025 – Current
  * Python library for generating biological synthetic data using ODE models to benchmark ML workflows
* **CANISR** | Project Lead, Sep 2025 – Current
  * Full-stack platform (Postgres/FastAPI/Next.js) for standardising and visualising drug response modelling

Skills
======
* **Technical Stack**: Python (Proficient), TypeScript, SQL, Docker, Postgres, Next.js, FastAPI, Scikit-learn
* **Specialties**: Mathematical modelling, machine learning, feature engineering, data visualisation


Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
  
Service, Awards and leadership
======
* Monash Graduate Excellence Scholarship, 2022
* Biomed Mentorship Program, 2019 
* Public Health Student Excellence Award, 2018 
