# LAF: Locally Adaptive Factor (LAF) processes for multivariate time series

This repository is associated with the paper Durante, Scarpa and Dunson (2014). [*Locally Adaptive Factor Processes for Multivariate Time Series*](http://jmlr.org/papers/v15/durante14a.html). Journal of Machine Learning Research, 15:1493—1522, and aims at providing **code and materials to implement the Gibbs sampler algorithm for the mean-covariance regression model described in the paper**. Readers interested in this topic can also refer to the NIPS paper Durante, Scarpa and Dunson (2013). [*Locally Adaptive Bayesian Multivariate Time Series*](http://papers.nips.cc/paper/5115-locally-adaptive-bayesian-multivariate-time-series). Advances in Neural Information Processing Systems, 26:1664—1672.

Here we provide the `Julia` implementation from [jmxpearson](https://github.com/jmxpearson), with additional comments and guidelines. For a version of the Gibbs sampler code in `Python` refer to the [original repository](https://github.com/jmxpearson/laf) from [jmxpearson](https://github.com/jmxpearson). Note that, for the `Julia` implementation, the following **dependencies**:

    - Julia 0.4+ (dev branch as of this testing)
    - PyPlot (for plotting)
    - Distributions

are required. 

The materials for the implementation can be found in the directory [julia](https://github.com/danieledurante/LAF/tree/master/julia), which provides the following files:

- **Modules to implement relevant routines required for the Gibbs sampler**.
    - `SStools.jl` implements the codes associated with the **Kalman filter and smoother**, as well as functions to draw samples from the **simulation smoother** of Durbin and Koopman (2002). [*A Simple and Efficient Simulation Smoother for State Space Time Series Analysis*](http://biomet.oxfordjournals.org/content/89/3/603.short). Biometrika, 89:603—615. These functions are fundamental to draw posterior samples from the state space approximation of the nested Gaussian Process (nGP) described in equations (3) and (4) of our paper.
    - `NGPtools.jl` implements the code to draw posterior samples from the **nested Gaussian Process (nGP)** of Zhu and Dunson (2013). [*Locally Adaptive Bayes Nonparametric Regression via Nested Gaussian Processes*](http://amstat.tandfonline.com/doi/abs/10.1080/01621459.2013.838568#.VdsWUNNViko). Journal of the American Statistical Association, 108:1445—1456.

- **Codes to generate the datasets required for the validation of the different routines and the final Gibbs sampler**.
    - `test_signals.ipynb` generates **test data with locally-varying smoothness** leveraging the examples in Donoho and Johnstone (1994). [*Ideal Spatial Adaptation by Wavelet Shrinkage*](http://biomet.oxfordjournals.org/content/81/3/425.short). Biometrika, 81:425—455. This code is in `Python`, and is useful to validate both the nGP code, and the Gibbs sampler for the LAF model.
    - `make_laf_test_data.ipynb` generates **test data from a time-varying factor process** to validate the LAF algorithm and saves the results in a jld (`HDF5`) file. This code is based on the constructive representation of the LAF described in Section 2 of our paper. **The resulting dataset is a useful benchmark to compare performance of other models or algorithms**.

- **Codes to validate the different routines**.
    - Kalman Filter and Simulation Smoother.
        - `kalman_integration_test.ipynb` contains a tutorial implementation to **valide the Kalman filter and smoother codes**.
        - `kalman_interleaved_integration_tests.ipynb` **validates alternative "univariate" Kalman filter and smoother algorithms** that collapse vector-valued observations into a single univariate time series. This can result in large speedups when the covariance matrix of the observation model is diagonal.
    - Nested Gaussian Process (nGP).
        - `integration_test_nGP_samples.ipynb` tests the ability of the nGP code to draw samples from a (2, 1) nested Gaussian Process.
        - `MCMC_integration_test.ipynb` contains a tutorial implementation to **valide the code for posterior inference under the nested Gaussian Process**. This tutorial, makes use of the data generated by `test_signals`.

- **Final code for the LAF Gibbs sampler and validation on a tutorial simulation**.
    - `laf_integration_test.ipynb` **implements the final Gibbs sampler for posterior inference under the LAF model, and validates it in a tutorial simulation** mimicking the one considered in the paper. This tutorial, makes use of the data generated by `make_laf_test_data.ipynb`.


