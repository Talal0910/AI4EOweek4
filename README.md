# AI4EOweek4
This repository contains the Week 4 assignment for GEOL0069: Artificial Intelligence for Earth Observation, focusing on the application of unsupervised learning methods to satellite altimetry data. The primary objective is to classify Sentinel-3 altimetry waveforms and distinguish between sea ice and leads.

The work builds upon the provided notebook Unit_2_Unsupervised_Learning_Methods.ipynb, extending it through feature engineering, clustering analysis, and waveform interpretation.

# Initial requirments
The code must be mounted to the required Google Drive
```python
from google.colab import drive
drive.mount('/content/drive')
```

The correct packages must also be installed
```python
!pip install msalign
!pip install rasterio
!pip install netCDF4
```

# Context
The Sentinel-3 mission measures sea surface topography to support studies of sea level, sea ice, ocean currents, waves, and winds. Altimetry satellites transmit radar pulses toward Earth and record the returning echo; the travel time of this signal is used to calculate surface elevation. Variations in surface properties affect the shape and strength of the returned echo, allowing us to distinguish between features such as sea ice and leads — the focus of this project.

# Gaussian Mixture Models
A Gaussian Mixture Model (GMM) is used to cluster Sentinel-3 altimetry data without predefined labels. GMMs work by modelling the data as a combination of several Gaussian distributions, allowing patterns in the feature space to be identified automatically.

The model uses the Expectation–Maximization (EM) algorithm to estimate how likely each data point is to belong to each cluster, and then adjusts the model parameters accordingly. The number of clusters and covariance type are defined before fitting the model.

Before clustering, waveform-derived features such as peakiness and stack standard deviation (SSD) are extracted and cleaned to ensure reliable results. 

# Results
The resulting clusters of functions can then be extracted and plotted. The echos in the sea ice cluster are been labelled '0' and the lead echos are labelled '1'
<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/9d2ccfc8-5c5f-4d6d-a7c2-7ec084eb37fb" />

Here are the means for the sea ice and lead echos:
<img width="560" height="435" alt="image" src="https://github.com/user-attachments/assets/d2575244-5836-4531-8f57-898282442e69" />

Here are the standard deviations for the sea ice and lead echoes:
<img width="560" height="435" alt="image" src="https://github.com/user-attachments/assets/6b47b44b-1f9e-4866-9286-2669b76d5fe4" />

# Noise Reduction and Waveform Alignment

Altimetry waveforms can contain small horizontal shifts in peak position caused by surface variability and instrument effects. These shifts introduce noise that can affect clustering results.

To reduce this variability, waveform alignment was applied using msalign, which centres the dominant peak of each waveform at a fixed reference bin. This ensures that clustering reflects differences in waveform shape rather than positional offsets, leading to more reliable separation of sea ice and leads.

After implementing msalign to reduce variability, the new mean and standard deviations are shown:
<img width="560" height="435" alt="image" src="https://github.com/user-attachments/assets/20715079-3b45-43a1-8295-ec38cb9b24b3" />

<img width="569" height="435" alt="image" src="https://github.com/user-attachments/assets/4b1ac72b-bc13-4726-8671-692536173316" />


# Comparing with the ESA
<img width="683" height="547" alt="image" src="https://github.com/user-attachments/assets/21681ad5-424c-4569-b020-4531874dae1f" />

The confusion matrix shows strong agreement between the GMM classification and the ESA reference. Out of 12,195 samples, only 46 were misclassified, resulting in an overall accuracy of approximately 99.6%.

Both sea ice and leads are identified with high reliability, indicating that the selected features effectively separate the two surface types. The small number of disagreements may occur at ice–lead boundaries or in cases where waveform characteristics are ambiguous.

# Contact

Talal Khan - ucfbtkh@ucl.ac.uk

# Acknowledgements

This project is part of an assignment for module GEOL0069 taught in the UCL Earth Sciences Department
