# CNN_Wavelet

**Author**: Fyodor Sviridov

This document presents the results of a project on the course "Artificial Intelligence".

**Abstract**.
To attempt to solve a clustering problem for a large set of gamma-ray burst light curves: to apply a wavelet transform to them, extract features from the resulting scalograms using a pre-trained convolutional neural network, and finally apply clustering algorithms for high-dimensional data.

**Data Preparation**.  
I downloaded gamma-ray burst data recorded by the Fermi/GBM telescope from January 1, 2020 to the present. During this period, approximately 1060 bursts were detected. From the TTE data, light curves were constructed with a time resolution of 0.128 seconds in the energy range of 50–300 keV. The time interval for all light curves was chosen from −100 s to 400 s relative to the trigger time. Light curves that did not contain data covering this time interval were discarded. _Min–max_ scaling was applied to the light curves.

**Wavelet Transform**.  
For the wavelet analysis, I used the PyWavelets module. The mother wavelet was chosen as ‘cmor1.5-1.0’, with a bandwidth of 1.5 and a central frequency of 1.0. All normalized light curves were transformed using the same set of scales, corresponding to a frequency range from 10⁻⁴ Hz to 9.5 Hz. The result of the transform is a matrix of wavelet power spectrum coefficients (|w|²). These matrices were also normalized using _min–max_ scaling.

**Convolutional Neural Network**.  
To analyze the resulting scalogram images, CLIP was used—a pre-trained CNN whose purpose is to associate any image with a text description of that image (https://github.com/openai/CLIP.git).  
The input image must be three-channel with a resolution of 224 × 224; therefore, the scalogram matrices were resized to this resolution and replicated across three channels.  
As a result, features were extracted from approximately 1000 normalized scalograms, each feature vector having a dimensionality of 512.

**Dimensionality Reduction**.  
I attempted to include additional light-curve parameters such as duration, spectral characteristics, luminosity, and redshift, but this did not provide significant additional information and did not strongly affect the final clustering. Therefore, I restricted the analysis to parameters obtained solely from the scalogram images.

To reduce the dimensionality of the resulting data, I applied principal component analysis (PCA), retaining 30 components, which corresponds to approximately 95% of the total variance.

**Clustering**.  
For clustering, I used DBSCAN, with the parameter *eps* determined by constructing the k-nearest-neighbor distance plot. To visualize the DBSCAN results, I used the t-SNE algorithm; this method was also employed as an alternative approach for identifying clusters. The obtained results and conclusions are presented below.

![image](/images/duration.png)
![image](/images/clusters.png)

**Conclusions**.  
The performed clustering analysis of gamma-ray bursts did not reveal well-defined structures, except for a weak separation into short and long bursts. This may be attributed to several factors:

- **Coarse data selection** — the bursts selected in this study exhibit certain limitations, and a more careful data selection procedure should be applied.
- **Initial feature set** — the extracted features may be insufficiently informative for identifying meaningful clusters. In future studies, it would be worthwhile to consider augmenting the scalogram-based features with additional parameters such as duration, spectral characteristics, luminosity, and redshift.
- **Wavelet choice** — since the results strongly depend on the choice of the mother wavelet, a comparative analysis of different wavelet families (Daubechies, Haar, Morlet, etc.) should be conducted.