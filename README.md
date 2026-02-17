<!-- ABOUT THE PROJECT -->
## About The Project
This project explores unsupervised learning methods for classification tasks. Specifically, we use K-means clustering and Gaussian Mixture Models (GMM) to discriminate between sea ice and leads using:
* Sentinel-2 optical data
* Sentinel-3 altimetry data
Although the notebook discusses both datasets, this README focuses primarily on the Sentinel-3 altimetry data.
Please refer to the full notebook for a comprehensive overview of the complete workflow.

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#Installation">About The Project</a>
      <ul>
        <li><a href="#">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#Installation">Usage</a></li>
    <li><a href="#Objectives">Roadmap</a></li>
    <li><a href="#Overview of Unsupervised Learning Methods">Contributing</a></li>
     <ul>
        <li><a href="#K-means Clustering">Prerequisites</a></li>
        <li><a href="#GMM">Installation</a></li>
      </ul>
    </li>
    <li><a href="#Why Use GMM for Echo Classification?">License</a></li>
    <li><a href="#Results">Contact</a></li>
    <ul>
        <li><a href="#Average">Prerequisites</a></li>
        <li><a href="#Standard Deviation">Installation</a></li>
        <li><a href="#Confusion Matrix">Installation</a></li>
      </ul>
    </li>
   
  </ol>
</details>
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
### GMM 
Assumes that the data is generated from a mixture of Gaussian distributions.
It assigns probabilities to each data point belonging to each cluster — a process known as soft clustering.

   ```sh
from sklearn.mixture import GaussianMixture
import matplotlib.pyplot as plt
import numpy as np

# Sample data
X = np.random.rand(100, 2)

# GMM model
gmm = GaussianMixture(n_components=3)
gmm.fit(X)
y_gmm = gmm.predict(X)

# Plotting
plt.scatter(X[:, 0], X[:, 1], c=y_gmm, cmap='viridis')
centers = gmm.means_
plt.scatter(centers[:, 0], centers[:, 1], c='black', s=200, alpha=0.5)
plt.title('Gaussian Mixture Model')
plt.show()

   ```
![GMM](gmm_plot.png)

#### Why Use GMM for Echo Classification?
Although both methods can be used for this task, GMM is generally preferred because:
* It provides probabilistic classification, giving insight into uncertainty
* It explicitly models variance within clusters
* Echo shapes for sea ice and leads can overlap

## Results
The differences in echo shape can be explained by basic radar physics.
A radar altimeter sends a microwave pulse towards the Earth's surface. The pulse reflects off either sea ice or open water and returns to the satellite, which records the returned signal as a waveform (echo).
![echoes](all_echos.png)
![Sea](sea_ice_clusters_echos.png)
![lead](lead_clusters_echos.png)
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
