# CNN_Wavelet

This project presents the results of an artificial intelligence study focused on clustering gamma-ray burst (GRB) light curves.

### Objective
The goal is to cluster a large set of GRB light curves by applying a wavelet transform, extracting features from the resulting scalograms using a pre-trained convolutional neural network, and performing clustering in a high-dimensional feature space.

### Data
GRB data recorded by the Fermi/GBM telescope from January 1, 2020 to the present were used (≈1060 bursts). Light curves were constructed from TTE data with a time resolution of 0.128 s in the 50–300 keV energy range, over the interval from −100 s to 400 s relative to the trigger time. Incomplete light curves were discarded, and min–max scaling was applied.

### Wavelet Analysis
Wavelet transforms were computed using PyWavelets with the complex Morlet wavelet `cmor1.5-1.0`. All light curves were transformed using the same set of scales, covering frequencies from 10⁻⁴ Hz to 9.5 Hz. The resulting wavelet power spectra were normalized using min–max scaling.

### Feature Extraction
Scalograms were resized to 224 × 224 and converted to three-channel images. Feature extraction was performed using the pre-trained CLIP convolutional neural network, producing 512-dimensional feature vectors for ~1000 scalograms.

### Dimensionality Reduction
Principal component analysis (PCA) was applied to the extracted features. The first 30 components, explaining about 95% of the total variance, were retained. Additional physical parameters were tested but did not significantly affect the clustering results.

### Clustering and Visualization
Clustering was performed using DBSCAN, with the `eps` parameter selected via a k-nearest-neighbor distance plot. The results were visualized using t-SNE, which was also explored as an alternative method for cluster identification.
