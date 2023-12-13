---
layout: page
title: "Optimization of Thermal Performance of Kagome Lattice Structure within Rectangular Channels"
description: "Revolutionizing aerothermal engineering with lattice structures for enhanced heat transfer and energy efficiency."
img: assets/img/Kagome.jpg
importance: 2
category: "Research Projects"
related_publications:
---

<strong>Time:</strong> Jul 2021 – Jul 2022

## Introduction

<strong>Lattice structures</strong>, typified by their geometrically intricate designs, are renowned for their capability to enhance heat transfer performance in areas subjected to high thermal conditions. <strong>The Kagome lattice structure, characterized by its distinctive trihexagonal tiling pattern, is gaining prominence in various engineering applications due to its unique thermal and mechanical properties.</strong> One of the primary challenges in the field of aerothermal engineering is the optimization of thermal performance within confined spaces, such as the trailing edge region of a turbine airfoil. Addressing this challenge, the research undertakes a comprehensive analysis of Kagome lattice structures within rectangular channels. By simulating conditions similar to the trailing edge region, the study emphasizes the potential of the Kagome lattice structure for enhanced energy efficiency and sustainability. Improved thermal management in turbine airfoils could lead to increased energy efficiency, thus lowering greenhouse gas emissions. The study is divided into two main objectives:

- Firstly, to establish a relationship between heat transfer performance and the geometric parameters of the Kagome lattice, such as streamwise distance (Sx), spanwise distance (Sy), and vertex spacing ratio (C/H).
- Secondly, to optimize this performance via genetic algorithms. The lattice structures are relevant for internal cooling systems in turbine airfoils, which face high thermal and mechanical stresses.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Kagome_Array.jpg" title="Kagome Array" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Kagome.jpg" title="Kagome Element" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: An array of the Kagome lattice structure in the rectangular channel. Right: Kagome element.
</div>

## Methods

<strong>The core innovation of this research encompasses a comprehensive optimization of the thermal characteristics of Kagome lattice structures.</strong> To achieve this, data was collected from 80 distinct experiments, each tailored to different geometric configurations of the Kagome lattice structures. The resulting dataset, comprising six independent and three dependent variables, forms the basis for the study, capturing key elements that impact the heat transfer properties.
Principal Component Analysis (PCA) was employed to make the dataset more manageable, a process through which the intrinsic structure of the data was preserved while simultaneously reducing its complexity, ensuring 95% of the variance from the original dataset remained intact.
A significant aspect of this research methodology involves regression models. These models provide a means to forecast outcomes based on certain conditions. The aim was to assess the reliability and accuracy of each model in predicting heat transfer outcomes given various Kagome lattice configurations. Four primary models were rigorously tested:

- **Linear Regression**
- **Decision Tree Regression**
- **Support Vector Regression**
- **Random Forest Regression**

The heat transfer performance could be optimized based on the predicted results. Here, genetic algorithms, inspired by natural evolutionary processes, were used to fine-tune the Kagome lattice configurations to reach peak thermal performance.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/Kagome_Methodology.jpg" title="Research Methodology" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The methodology of this project.
</div>

## Results

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Kagome_Result1.jpg" title="Regression Model Comparisons" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Kagome_Optimization.jpg" title="Optimization Flowchart" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: The result of regression model comparisons. Right: The optimization flowchart.
</div>

The regression models were assessed using metrics such as R2 value, Mean Square Error, and Mean Absolute Error. Based on this evaluation, the <strong>Random Forest Regression</strong> model demonstrated the best fit, with the highest R2 value of 0.92 and the lowest MAE/MSE in the four regression models. It was thus selected as the guiding model for the genetic algorithm optimization process. By applying Random Forest Regression as the fitness function in the genetic algorithm, the optimized Kagome lattice structure showed an overall 12% improvement in heat transfer efficiency across different Reynolds numbers—5000, 10000, and 15000—when compared to the best-performing experimental models.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/Kagome_Result2.jpg" title="Optimization Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The optimization results of this project.
</div>

## Discussion

This study provides robust insights into the optimization of thermal performance in Kagome lattice structures within rectangular channels. The research adheres to rigorous methodological standards and clearly outlines the objectives and outcomes. <strong>Specifically, the optimized Kagome lattice structures showed a 12% improvement in thermal efficiency.</strong> To put this in context, based on industry averages, a 12% improvement in thermal efficiency could lead to an estimated 6% reduction in energy consumption in turbine airfoil operations. As such, this research serves as a valuable steppingstone for future work in the field, including thermal applications in turbine airfoils and other high-stress thermal environments. A limitation of this research is the dataset size, which hindered the use of neural network models. Future work should explore larger datasets and consider the implementation of Bayesian optimization methods for further refinement.

