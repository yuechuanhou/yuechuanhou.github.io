---
layout: page
title: "Active Vision for Next Best View Planning in Outdoor Scenes"
description: "CMU 16-824 Course Project"
img: assets/img/NBV_Scan1GroundTruth.jpg
importance: 1
category: "Course Projects"
related_publications:
---

<strong>Time:</strong> Sep 22023 – Dec 2023

<strong>Skills:</strong> Computer Vsion, Python

## Abstract

In this project, we extend the exploration of autonomous robotic tasks in the context of larger outdoor scenes. Building upon the referenced work, we focus on planning views for these expansive environments, addressing questions related to optimal data collection given a set of reference images. A significant contribution of our approach lies in the introduction of a cutscene augmentation method. This innovative technique involves semantically dividing larger outdoor scenes into smaller components. Our model is then trained to predict uncertainty and RGB values for novel poses within these segmented scenes. This cutscene augmentation method serves a dual purpose. First, it effectively increases the size of the dataset by a significant percentage, enhancing the efficiency of the training process, and reducing overfitting on a scene. Second, and more importantly, it substantially increases the accuracy of novel view predictions. By leveraging this method, our project aims to overcome challenges associated with data collection in large-scale outdoor scenarios, providing a valuable contribution to the broader field of autonomous robotic tasks. Our experiments, using both synthetic and real-world data, demonstrate the effectiveness of our proposed uncertainty-guided approach. The results showcase improved accuracy in scene representations compared to baseline methods, validating the utility and generalizability of our methodology.

## Introduction and Related Work

Embodied robotic intelligence relies on active perception and exploration, essential for various applications like robotic manipulation, inspection, and vision-based navigation. The autonomous collection of data plays a pivotal role in scene understanding and subsequent tasks, with a focus on novel view synthesis using multiple UAVs to make the process more robust [1]. However, a significant challenge lies in efficiently planning a sequence of views for sensors, ensuring the acquisition of the most valuable information while adhering to platform-specific constraints and enhancing scene understanding for manipulation tasks [2]. Addressing this challenge is crucial for enhancing the training process, particularly in scenarios involving larger scenes.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_DataGathering.gif" title="Multi-UAV Data Gathering" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Manipulation.gif" title="Scene Understanding for Manipulation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Left: Multi-UAV Data Gathering. Right: Scene Understanding for Manipulation.</em>
</div>

## Contributions

In our research, we explored 3D scene understanding, focusing on larger outdoor environments represented by UrbanScene3D. We integrated this environment into Blender, generating datasets emulating camera poses and renders akin to the DTU dataset. We conduct a viewing range sensitivity analysis which involved various viewing ranges, creating datasets with different parameters, including a double dome configuration and maximum view angle changes aligned with specified view ranges of 30, 60, and 90 degrees. Additionally, we introduced cutscene augmentation, which augments that dataset by extracting more subscenes from each big scene, improving the utility of small datasets and giving the network more room to learn.

## Background

"NeRF" (Neural Radiance Fields) is a pioneering research paper in computer vision that presents a revolutionary method for 3D scene reconstruction and novel view synthesis. This approach employs neural networks to model volumetric scenes from 2D images, enabling the creation of immersive 3D environments and realistic rendering of novel viewpoints. NeRF has had a profound impact on various fields, including computer graphics, virtual reality, and robotics.

"PixelNeRF" is an extension of the NeRF framework, it attempts to generalize the NeRF network to multiple scenes by incorporating pixel level features from input reference views.This advancement further solidifies the potential of NeRF-based methods for tasks such as image synthesis, view interpolation, and scene reconstruction, making them valuable tools in computer vision and related domains.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <!-- Embedding the first video -->
        <iframe width="100%" height="315" src="https://www.youtube.com/embed/JuH79E8rdKc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        <div class="caption">
            <em>NeRF</em>
        </div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <!-- Embedding the second video -->
        <iframe width="100%" height="315" src="https://www.youtube.com/embed/voebZx7f32g" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        <div class="caption">
            <em>PixelNeRF</em>
        </div>
    </div>
</div>


## Approach

Our approach is based on the Next Best View Planning Using Uncertainty Estimation in Image-Based Neural Rendering (NeU-NBV) work by Jin which had been built on top of the pixelnerf framework. The NeU-NBV framework adapts pixel nerf to next best view planning using two techniques: 1. It enhances the speed of volumetric rendering through using an LSTM to predict the jumping distance in ray tracing. 2. It incorporates uncertainity estimation by pushing the network to output both a mean RGB value and a variance. The uncertainty and final RGB values are then extracted by sampling from the log normal distribution 100 times and taking the mean and variance of those samples.

Furthermore, the (NeU-NBV) work adds a planning framework on top of their adapted pixelnerf model as follows: Given two starting reference views, they sample multiple candidate views within a limited viewing range. They then feed all of those candidate views through the trained network which in turn outputs both an RGB and uncertainty prediction for each pixel. The next best view is then chosen such that it maximizes the information gain by targeting the novel view with the highest average pixel uncertainity. The novel view is then collected and the process is then repeated until the capture budget has been expended.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_NetworkArchitechture.jpg" title="Uncertainty Prediction Network Architechture" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_LossFunction.jpg" title="Uncertainity Estimation Loss Function" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_PlanningArchitechture.jpg" title="NBV Planning Architechture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>On the left, uncertainty prediction network architechture. Middle, uncertainity estimation loss function. Right, NBV planning architechture.</em>
</div>

## Method

Challenges arise when applying DTU pre-trained models for novel view prediction on larger outdoor navigational scenes, which it wasn't originally trained on. To address this, we embarked on an experimentation journey to develop a robust training strategy. We employed the UrbanScene3D environment, imported its .obj representation into Blender, and scripted a custom dataset generation process aligned with DTU's configuration. We also conducted a comprehensive sensitivity analysis to explore various viewing angles, adjusting scene parameters, such as varying radii for different configurations and introducing a range of maximum view angle changes. However, the complexity of larger scenes necessitates a more effective training strategy, prompting us to delve into innovative techniques like cutscene augmentation. This method involves dividing extensive scenes into manageable chunks, enhancing scene analysis and understanding for improved model performance.

For our research, we employed a larger outdoor navigational environment known as UrbanScene3D. To facilitate our experiments, we imported the .obj file representing this environment into Blender. Subsequently, we set up a custom script within Blender to generate a dataset. This dataset was designed to emulate camera poses and renders configured similarly to the DTU dataset, enabling us to conduct our experiments effectively and gather valuable insights from this rich outdoor environment.


<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_ViewCapture.gif" title="Training on Larger Outdoor Scenes" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Training on Larger Outdoor Scenes</em>
</div>


Utilizing the scene file within Blender, we conducted a comprehensive sensitivity analysis to explore various viewing angles. This approach allowed us to generate multiple datasets by manipulating the scene parameters. Specifically, we created a double dome setup within the scene, varying the radii for two different configurations. Moreover, we introduced a range of maximum view angle changes for each dataset, aligning the maximum change with the specified view range. This rigorous analysis encompassed view ranges of 30 degrees, 60 degrees, and 90 degrees, providing us with valuable datasets that capture a wide array of perspectives and visual information for our research.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_ViewRange.gif" title="Viewing Range Sensitivity Analysis" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Viewing Range Sensitivity Analysis</em>
</div>

Taking inspiration from simple yet effective 2D augmentation methods such as cutout and cutmix, and stemming from the observations that large scale outdoor scenes are scarce and such scenes are often comprised of many smaller subscenes, we introduce cutscene augmentation. The cutscene augmentation method increases the variation of the data by cutting out semantically divisible subscenes out of a larger scene. Those cutout scenes are scaled up and added as different data points pushing the network to generalize and avoid overfitting when data is scarce as it often is for large outdoor scenes.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Cutscene.gif" title="Cutscene Augmentation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Cutscene Augmentation</em>
</div>

## Experimental Setup

In our experimental setup, we employed a state-of-the-art approach for training our computer vision model. We utilized a high-performance computing environment with a 3090Ti GPU to accelerate our training process. Our training procedure involved optimizing a range of hyperparameters, including learning rates, and batch sizes, to achieve optimal performance. We conducted training for a total of 200 epochs to ensure that our model converged to a robust and accurate solution. This rigorous experimental setup allowed us to achieve state-of-the-art results and validate the effectiveness of our proposed approach.

## Viewing Range Sensitivity Analysis Results

Below, we show qualitative results by fixing the reference views such that they are at most 30 degrees apart and fixing the targetted novel view. We then vary the model used to make the rgb and uncertainty estimation for this fixed input.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_ReferenceImages.jpg" title="Reference Images" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Reference Images</em>
</div>

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0GroundTruth.jpg" title="Ground Truth" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Ground Truth</em>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="caption text-center">
            <strong>90 Degrees</strong>
        </div>
        {% include figure.html path="assets/img/NBV_Scan0Best90RGB.jpg" title="90 Degrees - RGB" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>RGB</em>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div class="caption text-center">
            <strong>60 Degrees</strong>
        </div>
        {% include figure.html path="assets/img/NBV_Scan0Best60RGB.jpg" title="60 Degrees - RGB" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>RGB</em>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div class="caption text-center">
            <strong>30 Degrees</strong>
        </div>
        {% include figure.html path="assets/img/NBV_Scan0Best30RGB.jpg" title="30 Degrees - RGB" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>RGB</em>
        </div>
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0Best90Uncertainty.jpg" title="90 Degrees - Uncertainty" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Uncertainty</em>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0Best60Uncertainty.jpg" title="60 Degrees - Uncertainty" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Uncertainty</em>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0Best30Uncertainty.jpg" title="30 Degrees - Uncertainty" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Uncertainty</em>
        </div>
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0Best90Error.jpg" title="90 Degrees - Error" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Error</em>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0Best60Error.jpg" title="60 Degrees - Error" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Error</em>
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Scan0Best30Error.jpg" title="30 Degrees - Error" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Error</em>
        </div>
    </div>
</div>

We further measure PSNR and SSIM as measures of reconstruction quality during training. The curves show the superiority of the quality as you reduce the viewing range.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_CombinedPSNR.jpg" title="PSNR" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>PSNR</em>
        </div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_CombinedSSIM.jpg" title="SSIM" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>SSIM</em>
        </div>
    </div>
</div>

The sensitivity analysis we conducted affirms that our framework is very sensitive to the viewing range. Having too big of a range will affect the stability of training and the network's capability to learn, while having too small of a range would limit the network's novel view estimation. Note that even though the reconstruction quality reduces with a bigger viewing range, the uncertainty still correlates well with the error which is desirable. Thus, such models can still be utilized to estimate the next best view allbeit less reliably as the uncertainty predictions would be highly noisy. An interesting approach to study would be to incrementally increase the viewing range during training to stabilize training while simultaneously pushing the network to expand its viewing range uncertainty estimation capabilities. We leave this idea for future work.

## Cutscene Augmentation Results

For qualitative results, we conducted a more challenging analysis in which we varied the view 90 degrees in the x and y direction smoothly from -45,-45 to +45,+45 centered on the top bird's eye view. We then fixed the input views to be [-45,-45],[-35,-35],[+35,+35],[+45,+45].

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_TownReferenceImages.jpg" title="Town Reference Images" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Reference Images</em>
</div>

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_TownGroundTruth.gif" title="Town Ground Truth" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <em>Ground Truth</em>
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
    <div class="caption text-center">
            <strong>60 Degrees</strong>
        </div>
        {% include figure.html path="assets/img/NBV_Town60RGB.gif" title="60 Degrees - RGB" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>RGB</em>
        </div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
    <div class="caption text-center">
            <strong>60 Degrees + Cutscene</strong>
        </div>
        {% include figure.html path="assets/img/NBV_Town60CutsceneRGB.gif" title="60 Degrees + Cutscene - RGB" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>RGB</em>
        </div>
    </div>
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Town60Uncertainty.gif" title="60 Degrees - Uncertainty" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Uncertainty</em>
        </div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_Town60CutsceneUncertainty.gif" title="60 Degrees + Cutscene - Uncertainty" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>Uncertainty</em>
        </div>
    </div>
</div>

We again measure PSNR and SSIM as measures of reconstruction quality during training with cutscene augmentation.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_CombinedCutscenePSNR.jpg" title="Cutscene PSNR" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>PSNR</em>
        </div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_CombinedCutsceneSSIM.jpg" title="Cutscene SSIM" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            <em>SSIM</em>
        </div>
    </div>
</div>

The performance improvements introduced by a very simple augmentation technique such as cutscene are significant. Even though the model has never seen more than 60 degrees of viewing range in a single datapoint. It was able to generalize to a broader viewing range (90 degrees) surpassing the baseline. The visualization further serves to show how high uncertainity can hinder the NBV planning capabilities of the model. On the right side, we can trivially observe that the cutscene model's uncertainity is maximized at the birds eye view [0,0] whearas on the left side for the baseline, it is unclear visually which is the next best view as it suffers from high amounts of noise.

## Next Best View Planning Results

To test out planning performance, we took the trained model and tested planning. We start with a random set of two views, sample from all available ground truth views and choose the next best view based on three different policies. We report our model as "ours", the DTU pretrained model as "DTU_baseline", and the max view distance. We iterate until 6 views are collected and we measure the improvement of reconstruction with each new view collected using PSNR and SSIM as before.

<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/NBV_30Planning.jpg" title="Planning Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

As shown above, measured in reconstruction quality Our method outperformes the geometric and DTU trained baselines successfully extending the NeU-NBV framework to large outdoor scenes.

## Conclusion and Future Work

To conclude, in this work we have explored the application of NeU-NBV planning to large outdoor scenes. We conducted an evaluation on the sensitivity of the approach to viewing angle range and showed that the method is highly sensitive. We further introduced and evaluated a novel augmentation technique "cutscene". We demonstrated how cutscene can significantly enhance the performance on outdoor large scenes. In the future, we plan to further expand on the idea of cutscene possibly automating the approach of semanticly dividing the scene into multiple subcenes by utilizing a 2D birds eye view building/POI detector or perhaps using geometric clustering clustering the floor vertices and exposing buildings. Another interesting direction would be to add on to this augmentation technique by rearranging the cutout scenes into novel configurations pushing the variety of the scarce scene data we have even further. Furthermore, our viewing range sensitivity analysis hints at the possiblity that the network could benefit from a scheduled viewing range in which we increase the range gradually as the network learns.