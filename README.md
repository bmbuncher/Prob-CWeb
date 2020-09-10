# Probabilistic cosmic web classification using fast-generated training data
## Supplementary Material

Brandon Buncher<sup>1</sup> and Matias Carrasco Kind<sup>2,3</sup>

<sub><sup><sup>1</sup>Department of Physics, University of Illinois, Urbana IL 61801 USA</sup></sub>  
<sub><sup><sup>2</sup>Department of Astronomy, University of Illinois, Urbana IL 61801 USA</sup></sub>  
<sub><sup><sup>3</sup>National Center for Supercomputing Applications, Urbana IL 61801 USA</sup></sub>  


### This repository contains supplementary material for Probabilistic cosmic web classification using fast-generated training data (full article: https://doi.org/10.1093/mnras/staa2008 or preprint: https://arxiv.org/abs/1912.04412)

For the reuse of any of these files, please cite the MNRAS version of the article.

## Summary

We describe a novel trained machine learning algorithm we developed to classify individual cosmic web particles as members of halos/clusters, filaments, and voids.  We trained a random forest algorithm using a "fast-generated" dataset.  This dataset was designed to approximate the large-scale features of the cosmic web using pre-determined generation algorithm.  While this model lacks physical detail, it can be used to generate training data at a fraction of the cost of an N-body simulation without affecting classification accuracy.  This algorithm is described in detail in Section 2.1, and we demonstrate the robustness of its predictions in in Section 4.

Using calculations of the local density manitude and density field direcitonality, this algorithm assigns class probabilities to individual particles in a three-dimensional particle field.  The algorithm is highly scalable and customizable, lending its use to very large datasets and observed data.

In this repository, we have included supplementary figures relevant to the article.  These figures include:

- Alternative views of each particle field shown in the article.
- Figures showing the results of metric sets not discussed article.


Note that some of these plots are included in the paper directly or with altered formatting; however, we chose to include these figures here to allow for easier comparison of the different metric sets.

We have also included a demonstration of the filament generation algorithm.

# Key names, abbreviations, and directory structure

## Training metric abbreviations

For each particle (referred to here as the *central particle*) in the training dataset and *SIM*/*TSIM*, a set of measurements was performed.  These measurements were used to generate training data and make class predictions.  Save for *VOR* and *KNN* calculations, the measurements performed used all particles within a radius *R* of the central particle.  In addition, all measurements save for *PCA* measured the density field magnitude.  Additional details are provided in Section 2.2 and Table 2; this information is summarized here for convenience.

| Abbreviation | Name / Description |
|:------------:| ------------------ |
| *VOR*        | Voronoi Cell Volume: a Voronoi diagram of all particles in the dataset was created.  This measured the volume of the cell associated with each *central particle* |
| *CMD*        | Center of mass distance: the distance between the *central particle* and the center of mass of all othera particles within a radius *R* of the *central particle*. |
| *MI*         | Moment of inertia: value of the moment of inertia for all particles within a radius *R* of the *central particle*. |
| *ENC*        | Number enclosed: number of particles located within a radius *R* of the *central particle*. |
| *KNN*        | Distance between the central particle and its *k*th nearest neighbor |
| *PCA*        | Principal component analysis explained variance ratio: a PCA decomposition was performed on the positions of all particles within a radius *R* of the *central particle*.  The difference between the largest and smallest explained variance ratios was used to measure the density field directionality.  For more information on PCA decomposition, see (1) and (2) |

## Directory contents

### train

- Contains figures showing the training dataset with and without void particles.  The way that this dataset was generated is described in Section 2.1.

### test_true

- Contains figures showing the toy model prediction dataset (referred to as TSIM) with and without void particles.  The coloration corresponds with the true class values inherited from its creation algorithm.  The way that this dataset was generated is described in Section 2.1.

### toy_to_SIM

- Contains figures showing the predictions made by the random forest algorithm on SIM, an N-body simulation with 256^3 particles (described in detail in Section 2 and Table 2).  These predictions are described in Section 3.1.

### toy_to_TSIM

- Contains figures showing the predictions made by the random forest algorithm on TSIM.  These predictions are described in Section 3.2.


## File naming conventions / reason for inclusion

### \<n/>NN
- These figures contain predictions made by an algorithm trained with only *KNN* calculations for *k* <= *n*.
  
- The goal of these were to demonstrate that *KNN* is an effective measure of the density magnitude, as well as how its effectiveness changes with larger *k*-values.

### \<n/>NN_PCA
- These figures contain predictions made by an algorithm trained with only *KNN* and *PCA* calculations for *k* <= *n*.
  
- The goal of these were to demonstrate that *PCA* calculations in tandem with *KNN* improves the robustness of our predictions, particularly for filaments, and lessens the dependence of our predictions on the maximum *k*-value.

### ALL
- These figures contain predictions made by an algorithm trained using all metrics at all *R*/*k*-values.
  
- This metric set was primarily used for comparison with other data sets.

### CMD, MI, ENC
- These figures contain predictions made by an algorithm trained using only the specified feature.  These calculations used all *R*-values.
  
- The goal of these were to demonstrate that *CMD*, *MI*, and *ENC* are ineffective measures of the density magnitude.  The measurement histograms in particular show little differentiation between the LSS classes, providing a possible explanation for their ineffectiveness.

### VOR
- These figures contain predictions made by an algorithm trained using only *VOR*.
  
- These show that *VOR* was an ineffective measure of the density magnitude.  This is especially apparent in the HMF and ROC AUC.

### PCA
- These figures contain predictions made by an algorithm trained using only the *PCA* calculations at all *R*-values.
  
- The poor results, particularly of the HMF and ROC AUC, show that directionality calculations alone are insufficient for generating robust class predictions.;

### VOR_PCA
- These figures contain predictions made by an algorithm trained using only *VOR* and *PCA* using all possible *R*-values for the *PCA* calculations.
  
- The goal of these were to demonstrate that, even with directionality calculations, *VOR* was a poor proxy for local density magnitude.


## Subdirectory structure

### HMF, meas_hist, ROC
- Contain plots of the HMFs, measurement histograms, and ROC curves for each metric set, respectively.  These plots are discussed in greater detail in Sections 3.1, 3.2, and 4.2.

### Prob_Cont and Prob_Cont_Vector
These are probability contrast fields for the predictions made by the algorithm trained using a given metric set.  These show the degree to which a given metric set was able to differentiate between classes.  **Prob_Cont** contains PNG images, while **Prob_Cont_Vector** contains vector PDF images, which are substantially larger due to the number of particles in each image.

- **HF**: each particle was classified as a halo or filament particle, and the coloration for a given particle corresponds with the halo probability assigned to it minus the filament probability; a positive value (corresponding to light blue coloration) indicated that the algorithm assigned the particle a higher probability of being a halo particle than of being a filament particle; a negative value (orange coloration) indicates that the particle was more likely to be a filament member than a halo member; and a value near zero (black coloration) indicates that the particle's class was ambiguous.

   - Of note, *CMD*, *MI*, and *ENC* generally assigned particles extreme probability difference values to particles (1, -1, or 0), possibly indicating the existence of an implicit density magnitude cutoff.  *KNN* exhibited less extreme values, though the probabilities assigned were dependent on the maximum *k*-value used.  The addition of *PCA* calculations improved robustness by lessening the strength of this dependence.

- These plots are discussed in detail in Sections 3.1, 3.2, and 4.4.

- **HV**: each particle was classified as a halo or void particle, and the coloration for a given particle corresponds with the halo probability assigned to it minus the void probability.  These plots were not discussed in the article.
  
  - Virtually all particles were assigned a probability difference of 1 or -1, indicating that, as expected by the stark difference in visual appearance between the structures, the algorithm was able to easily distinguish between these structures.

- **FV**: each particle was classified as a filament or void particle, and the coloration for a given particle corresponds with the filamet probability assigned to it minus the void probability.  These plots were not discussed in the article.

  - As in **HV**, virtually all particles were assigned a probability difference of 1 or -1, allowing the same conclusions to be drawn.



## References

(1) Pedregosa F., et al., 2011, Journal of Machine Learning Research, 12, 2825

(2) Tipping M. E., Bishop C. M., 1998, Mixtures of Probabilistic Principal Component Analysers

For a full list of references used, please see the article.


## Acknowledgements

This material is based upon work supported by the National Science Foundation Graduate Research Fellowship Program under Grant No. DGE --- 1746047

The second author's work has been supported by grant projects NSF AST 07-15036 and NSF AST 08-13543.

This research is part of the Blue Waters sustained-petascale computing project, which is supported by the National Science Foundation (awards OCI-0725070 and ACI-1238993) and the state of Illinois. Blue Waters is a joint effort of the University of Illinois at Urbana-Champaign and its National Center for Supercomputing Applications.
