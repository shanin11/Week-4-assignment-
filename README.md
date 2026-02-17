<!-- ABOUT THE PROJECT -->
## About The Project
This project explores unsupervised learning methods for classification tasks. Specifically, we use K-means clustering and Gaussian Mixture Models (GMM) to discriminate between sea ice and leads using:
* Sentinel-2 optical data
* Sentinel-3 altimetry data
Although the notebook discusses both datasets, this README focuses primarily on the Sentinel-3 altimetry data.
Please refer to the full notebook for a comprehensive overview of the complete workflow.

## Installation

## Objectives
The main objectives of this notebook are to use GMM to:
* Classify radar echoes as either sea ice or lead
* Produce an average echo waveform for each class
* Calculate the standard deviation for both classes
* Evaluate classification performance against the official ESA classification using a confusion matrix

## Overview of Unsupervised Learning Methods
### K-means Clustering
K-means clustering divides the data into a predefined number of clusters by assigning each data point to the nearest cluster centre based on similarity.
Gaussian Mixture Models (GMM)
### GMM assumes that the data is generated from a mixture of Gaussian distributions.
It assigns probabilities to each data point belonging to each cluster — a process known as soft clustering.
Insert code for setup and plotting here.

#### Why Use GMM for Echo Classification?
Although both methods can be used for this task, GMM is generally preferred because:
* It provides probabilistic classification, giving insight into uncertainty
* It explicitly models variance within clusters
* Echo shapes for sea ice and leads can overlap

## Results
The differences in echo shape can be explained by basic radar physics.
A radar altimeter sends a microwave pulse towards the Earth's surface. The pulse reflects off either sea ice or open water and returns to the satellite, which records the returned signal as a waveform (echo).
![echoes](all_echoes.png)
![Sea](sea_ice_clusters_echoes.png)
![lead](lead_clusers_echoes.png)
The shape of the echo waveform depends on surface type:
* Sea Ice: A rough, uneven surface produces a broader, more spread-out echo.
* Lead: A smooth, flat surface produces a stronger, sharper echo.
![Mean](average_echo_shape.png)


## Standard Deviation Analysis
![STD](std_dev_echo_shape.png)
The standard deviation shows how much the waveforms vary around the mean.
* Sea ice:The standard deviation is relatively low and smooth. There is a moderate peak around the main echo return, but variability remains small beyond this region.
* Lead:The standard deviation shows a sharp and high peak around the main return. Variability is significantly larger at the peak, reflecting fluctuations in surface smoothness and specular reflection strength.
These findings demonstrate why GMM is well-suited for this task: it explicitly models variance, whereas K-means may struggle to capture differences in variability between clusters.
Comparing the mean and standard deviation waveforms provides a clearer understanding of the distinguishing features between sea ice and leads, which is crucial for reliable classification.

## Confusion Matrix
![CM](confusion_matrix.png)
This section evaluates classification performance against the official ESA labels and quantifies agreement using accuracy and error metrics

<p align="right">(<a href="#readme-top">back to top</a>)</p>
