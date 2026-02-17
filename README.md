<!-- ABOUT THE PROJECT -->
## About The Project
This project explores unsupervised learning methods for classification tasks. Specifically, we use K-means clustering and Gaussian Mixture Models (GMM) to discriminate between sea ice and leads using:
* Sentinel-2 optical data
* Sentinel-3 altimetry data

Although the notebook in this repository discusses both datasets, this README focuses primarily on the Sentinel-3 altimetry data.

Please refer to the full notebook for a comprehensive overview of the complete workflow. 
You can find this in the main repository in the file Unit_2_Unsupervised_Learning_Methods.ipynb. This notebook was built using Google Colab and builds on Chapter1_Unsupervised_Learning_Methods_Michel.ipynb.

<!-- TABLE OF CONTENTS -->
#### Table of Contents
<ul>
  <li><a href="#installation">Installation</a></li>
  <li><a href="#objectives">Objectives</a></li>

  <li>
    <a href="#overview-of-unsupervised-learning-methods">
      Overview of Unsupervised Learning Methods
    </a>
    <ul>
      <li><a href="#k-means-clustering">K-means Clustering</a></li>
      <li><a href="#gaussian-mixture-models-gmm">Gaussian Mixture Models (GMM)</a></li>
      <li><a href="#why-use-gmm-for-echo-classification">Why Use GMM for Echo Classification</a></li>
    </ul>
  </li>

  <li>
    <a href="#results">Results</a>
    <ul>
      <li><a href="#echo-shape-and-averages">Echo Shape and Averages</a></li>
      <li><a href="#standard-deviation-analysis">Standard Deviation Analysis</a></li>
      <li><a href="#confusion-matrix">Confusion Matrix</a></li>
    </ul>
  </li>

  <li><a href="#acknowledgements">Acknowledgements</a></li>
</ul>


## Installation
You need to install the following packages for this project.
 ```sh
  !pip install rasterio
  !pip install netCDF4
  ```

## Objectives
The main objectives of this project is to use unsupervised learning methods to:
* Classify radar echoes as either sea ice or lead
* Produce an average echo waveform for each class
* Calculate the standard deviation for both classes
* Evaluate classification performance against the official European Space Agency (ESA) classification using a confusion matrix

## Overview of Unsupervised Learning Methods
### K-means Clustering
K-means clustering divides the data into a predefined number of clusters by assigning each data point to the nearest cluster centre based on similarity.
### Gaussian Mixture Models (GMM)
GMM assumes that the data is generated from a mixture of Gaussian distributions.
It assigns probabilities to each data point belonging to each cluster, a process known as soft clustering. 

The code and output for implementing GMM is shown below.

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
The differences in echo shape between sea ice and lead can be explained by basic radar physics.
A radar altimeter sends a microwave pulse towards the Earth's surface. The pulse reflects off either sea ice or open water (lead) and returns to the satellite, which records the returned signal as a waveform (echo).
### Echo Shape and Averages
The plots of all the echoes, sea ice cluser echoes and lead echoes are shown below.
![echoes](all_echos.png)
![Sea](sea_ice_clusters_echos.png)
![lead](lead_clusters_echos.png)
The shape of the echo waveform depends on surface type:
* Sea Ice: A rough, uneven surface produces a broader, more spread-out echo.
* Lead: A smooth, flat surface produces a stronger, sharper echo.

We can see these patterns in the echo plots above and the average echo plot below.
![Mean](average_echo_shape.png)


### Standard Deviation Analysis
![STD](std_dev_echo_shape.png)
The standard deviation shows how much the waveforms vary around the mean.
* Sea ice: The standard deviation is relatively low and smooth. There is a moderate peak around the main echo return, but variability remains small beyond this region.
* Lead: The standard deviation shows a sharp and high peak around the main return. Variability is significantly larger at the peak, reflecting fluctuations in surface smoothness.

These findings also demonstrates why GMM is well-suited for this task: it explicitly models variance, whereas K-means may struggle to capture differences in variability between clusters.

Comparing the mean and standard deviation waveforms provides a clearer understanding of the distinguishing features between sea ice and leads, which is crucial for reliable classification.

### Confusion Matrix
![CM](confusion_matrix.png)

This section evaluates classification performance against the official ESA labels and quantifies agreement using accuracy and error metrics

## Acknowledgments
This project is part of an assignment for the GEOL0069 module taught in UCL Earth Sciences Department.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
