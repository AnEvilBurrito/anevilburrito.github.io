---
title: "Engineering network-based dynamic features to predict drug sensitivity"
collection: talks
type: "Talk"
permalink: /talks/2024-network-engineering-drug-response
venue: "Peter MacCallum Cancer Centre, Melbourne"
date: 2024-06-25
location: "Melbourne, Victoria (Australia)"
---

Computational discovery of cancer drug response biomarkers typically employs machine learning
pipelines to analyse large pharmacogenomic datasets. However, these -omics datasets, while extensive,
lack critical information about intracellular protein signalling dynamics, which can determine drug
response outcomes. The static nature of these datasets thus limits their ability to predict drug response
mechanistically.

Here, we present a method to engineer 'dynamic' features from established Ordinary Differential Equation
(ODE) models of cancer signalling networks. These ODE models incorporate mechanistic biological
knowledge as kinetic equations based on biochemical principles, such as Michaelis–Menten kinetics.
Using transcriptomic data, the ODE model is individualised to specific cell lines, simulating protein
dynamics based on the cell line's transcriptomic profile. Dynamic features are then computed from the
simulation data using mechanistic rules to capture the characteristics of the generated protein dynamics
time-series.
