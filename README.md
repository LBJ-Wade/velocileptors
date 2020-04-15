# velocileptors
Velocity-based LPT and EPT expansions of redshift-space distortions and
velocity statistics.

This code computes the real- and redshift-space power spectra and
correlation functions of biased tracers using 1-loop perturbation
theory (with effective field theory counter terms and up to cubic
biasing) as well as the real-space pairwise velocity moments.

The code requires numpy, scipy and pyFFTW (the python wrapper for FFTW):

https://hgomersall.github.io/pyFFTW/

to run. Note that pyFFTW is pip installable and available from conda-forge.

An example calculation to reproduce the plots in the paper is given
in "Moment Expansion Example.ipynb".
A short notebook detailing the same steps for the Fourier Streaming Model
is given in "Fourier Streaming Model Example.ipynb".
Also included is "Gaussian Streaming Model Example.ipynb" that runs through
how to produce the correlation function multipoles.
An example of the most common use-cases is given in "lpt_examples.py".

For most situations computing the power spectrum wedges or multipoles
is as simple as:

```
from   moment_expansion_fftw import MomentExpansion

mome        = MomentExpansion(klin,pklin,shear=True,threads=nthreads)
kw,pkw      = mome.compute_redshift_space_power_moments(pars,f,mu,reduced=True)
kl,p0,p2,p4 = mome.compute_redshift_space_power_multipoles(pars,f,reduced=True)
```


The core rsd modules are:

(1) moment_expansion_fftw.py: this computes redshift-space power spectra using the moment expansion approach.

(2) gaussian_streaming_model_fftw.py: this computes redshift-space correlation functions using the Gaussian streaming model.

(3) fourier_streaming_model_fftw.py: this computes redshift-space power spectra using the Fourier streaming model. 

All the rsd models use velocity spectra from velocity_moments_fftw.py, which itself inherits the real-space power spectrum module cleft_fftw.py.

All the above "physics" modules take in bias vectors given by 

bvec = [b1, b2, bs, alpha, alpha_v, alpha_s0, alpha_s2, sn, sv, s0]

where the parameters are:

(1) b1, b2, bs: the bias parameters up to quadratic order

(2) alpha, alpha_v, alpha_s0, alpha_s2: the one-loop counterterms for each velocity component

(3) sn, sv, s0: the stochastic contributions to the velocities


or (if you are interested in just the redshift-space power spectrum rather
than all of the velocity statistics individually) a _reduced set_ of
parameters is available:

(1) b1, b2, bs:  the bias parameters up to quadratic order

(2) alpha0, alpha2, alpha4: counter terms for ell=0, 2 and 4.

(3) sn, sn2: stochastic contributions to P_real(k) and sigma^2
    [e.g. shot-noise and finger-of-god dispersion].



In addition there are a few supporting modules:

(1) qfuncfft.py: a class to compute LPT kernels using 1d-ffts

(2) spherical_bessel_transform.py and spherical_bessel_transform_fftw.py: two fftlog routines, one that uses fftw and the other numpy

(3) loginterp.py: a function for performing log-extrapolated interpolating functions.

This code is related to the configuration-space based code https://github.com/martinjameswhite/CLEFT_GSM.
