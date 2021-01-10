<!-- Latex generated by https://jsfiddle.net/8ndx694g/ -->

# [Physics-aware] low-rank registration based manifold[/auto-encoder] for convection dominated PDEs

#### [[project website]](https://arxiv.org/abs/2006.15655)
<img src="data/schematic.png" width="250">

## Table of contents
* [Introduction](#introduction)
* [Method](#Method)
    * [Video-description](#Video-description)
* [FAQ](#FAQ)
* [Requirements](#Requirements)
* [Experiments](#Experiments)
    * [Rotating A](#Rotating-A)
    * [Two-dimensional fluid flows](#Two-dimensional-fluid-flows)
    * [Physics aware auto-encoder in an LSTM architecture](#Physics-aware-auto-encoder-in-an-LSTM-architecture)
* [Citation](#citation)

## Introduction
We design a physics-aware auto-encoder to specifically reduce the dimensionality of solutions arising from convection-dominated nonlinear physical systems. Although existing nonlinear manifold learning methods seem to be compelling tools to reduce the dimensionality of data characterized by a large Kolmogorov n-width, they typically lack a straightforward mapping from the latent space to the high-dimensional physical space. Moreover, the realized latent variables are often hard to interpret. Therefore, many of these methods are often dismissed in the reduced order modeling of dynamical systems governed by the partial differential equations (PDEs). Accordingly, we propose an auto-encoder type nonlinear dimensionality reduction algorithm. The unsupervised learning problem trains a diffeomorphic spatio-temporal grid, that registers the output sequence of the PDEs on a non-uniform parameter/time-varying grid, such that the Kolmogorov n-width of the mapped data on the learned grid is minimized. We demonstrate the efficacy and interpretability of our approach to separate convection/advection from diffusion/scaling on various manufactured and physical systems.

## Method

### Video-description 
- [Low-rank registeration based manifolds](https://youtu.be/fDYPAj9WAbk)

## FAQ

- **Why is the method considered *physics-aware*?**
	- The existance of a low-rank time/parameter-varying grid that minimizes the Kolmogrov n-width of the snapshots is a conjecture based on the physics of many of the convection dominated flows and is based on the possiblity of low-rank approximation of the characteristics line of hyperbolic PDEs, read sec 3.1 of [this](https://arxiv.org/abs/1701.04343).
	
- **Why is the method considered an *auto-encoder*?**
	- We make a one-to-one comparison of the traditional definition of a neural network-based auto-encoder to the proposed approach. An auto-encoder is defined as:
	<!-- \begin{array}{l}
		\phi: \mathcal{X} \rightarrow \mathcal{F} \\
		\psi: \mathcal{F} \rightarrow \mathcal{X} \\
		\phi, \psi=\underset{\phi, \psi}{\arg \min }\|X-(\psi \circ \phi) X\|^{2}.
		\end{array} -->
	
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://render.githubusercontent.com/render/math?math=%5Cbegin%7Barray%7D%7Bl%7D%0A%5Cphi%3A%20%5Cmathcal%7BX%7D%20%5Crightarrow%20%5Cmathcal%7BF%7D%20%5C%5C%0A%5Cpsi%3A%20%5Cmathcal%7BF%7D%20%5Crightarrow%20%5Cmathcal%7BX%7D%20%5C%5C%0A%5Cphi%2C%20%5Cpsi%3D%5Cunderset%7B%5Cphi%2C%20%5Cpsi%7D%7B%5Carg%20%5Cmin%20%7D%5C%7CX-(%5Cpsi%20%5Ccirc%20%5Cphi)%20X%5C%7C%5E%7B2%7D.%0A%5Cend%7Barray%7D">
	
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;It comprares to low-rank registeration based manifold defined as:
	<!-- \begin{array}{l}
		\mathcal{G}: {M} \rightarrow \widetilde{M} \\
		\mathcal{G}^{-1}: \widetilde{M} \rightarrow {M} \\
		\mathcal{G}, \mathcal{G}^{-1}=\underset{ \mathcal{G}, \mathcal{G}^{-1}}{\arg \min }\| M -\mathcal{G}^{-1} ( \mathcal{G} (M)) \|^{2}.
		\end{array} -->
		
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://render.githubusercontent.com/render/math?math=%5Cbegin%7Barray%7D%7Bl%7D%0A%5Cmathcal%7BG%7D%3A%20%7BM%7D%20%5Crightarrow%20%5Cwidetilde%7BM%7D%20%5C%5C%0A%5Cmathcal%7BG%7D%5E%7B-1%7D%3A%20%5Cwidetilde%7BM%7D%20%5Crightarrow%20%7BM%7D%20%5C%5C%0A%5Cmathcal%7BG%7D%2C%20%5Cmathcal%7BG%7D%5E%7B-1%7D%3D%5Cunderset%7B%20%5Cmathcal%7BG%7D%2C%20%5Cmathcal%7BG%7D%5E%7B-1%7D%7D%7B%5Carg%20%5Cmin%20%7D%5C%7C%20M%20-%5Cmathcal%7BG%7D%5E%7B-1%7D%20(%20%5Cmathcal%7BG%7D%20(M))%20%5C%7C%5E%7B2%7D.%0A%5Cend%7Barray%7D">

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Our proposed low-rank registeration based method *acts* as an auto-encoder where the *encoder/decoders*,<img src="https://render.githubusercontent.com/render/math?math=%5Cphi"> and <img src="https://render.githubusercontent.com/render/math?math=%5Cpsi">, are the mapping *to/from* the time/parameter-varying grid, <img src="https://render.githubusercontent.com/render/math?math=%5Cmathcal%7BG%7D"> and <img src="https://render.githubusercontent.com/render/math?math=%5Cmathcal%7BG%7D%5E%7B-1%7D">. The *code* is the interpolated data on the time/parameter-varying grid, here <img src="https://render.githubusercontent.com/render/math?math=%5Cwidetilde%7BM%7D">. The feature space, <img src="https://render.githubusercontent.com/render/math?math=%5Cmathcal%7BF%7D">, is the space of time/parameter-varying grid, i.e. <img src="https://render.githubusercontent.com/render/math?math=X%2BU_xV_x">. The proposed feature space is *compressed* since it is of a lower dimensionality (here rank) compared to the the input space, <img src="https://render.githubusercontent.com/render/math?math=%5Cmathcal%7BX%7D">, here <img src="https://render.githubusercontent.com/render/math?math=%5Cwidetilde%7BM%7D"> is of lower rank compared to <img src="https://render.githubusercontent.com/render/math?math=M">.
	
- **How does the method handle noisy data?**
	- SVD (singular value decomposition) and truncation at the heart of the algorithm acts as a filter removing the low energy-containing features of the data, i.e. the noise is filtered as a result of SVD-truncate.

- **What interpolation scheme to use?**
	- Use any of the off-the-shelf interpolation schemes, e.g. linear, cubic, spline. High order interpolation schemes only becomes advantageous in higher rank of reconstruction, i.e. higher <img src="https://render.githubusercontent.com/render/math?math=k_r">. This is due to the local aliasing of high wave-number bases (features) on the coarsened grid. Since we are often interested in a low-rank reconstruction, linear interpolation would be sufficient.

- **What optimization scheme to use?**
	- Use any of the methods that can handle nonlinear constraints.
	
## Requirements
- Matlab R2016+
- python 3.6
	- [scipy](https://pypi.org/project/scipy/)
	- [numpy](https://pypi.org/project/numpy/)
- [TensorFlow 2](https://www.tensorflow.org/install)
- [Keras 2.3.1](https://pypi.org/project/Keras/)

## Experiments
### Rotating A
Rotating A [Rotated A Location](./Experiments/rotatedA) 

open matlab
```
matlab -nodisplay -nosplash
```

Run the manifold learning
```
run main_rotatingA.m
```

Evaluate the snapshots on the learned manifold
```
run post_process.m
```

### Two-dimensional fluid flows
2D Fluid Flows (2D Riemann) [2D Fluid Flows](./Experiments/2DRiemann) 

open matlab
```
matlab -nodisplay -nosplash
```

Run the manifold learning
```
run main_opt_config03.m
run main_opt_config12.m
```
Evaluate the snapshots on the learned manifold
```
main_solve_config03.m
main_solve_config12.m
```


### Physics aware auto-encoder in an LSTM architecture
LSTM architecture [LSTM architecture](./Experiments/LSTM) 
* The notebooks are Google colab ready, make sure to have .py, *.pkl, *.h5 files at the same directory of the notebooks (*.ipynb)

#### Burgers' equation [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](./Experiments/LSTM/Burgers/main_manifold_burgers.ipynb)

Run the manifold learning and train the LSTM with Physics-aware registration based auto-encoder
```
main_manifold_burgers.ipynb
```

Run the manifold learning and train the LSTM with Neural Network based auto-encoder
```
main_eulerian_burgers.ipynb
```

#### Wave equation [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](./Experiments/LSTM/Wave/main_manifold_wave_small.ipynb)

Run the manifold learning and train the LSTM with Physics-aware registration based auto-encoder
```
 main_eulerian_wave_small.ipynb
```

Run the manifold learning and train the LSTM with Neural Network based auto-encoder
```
main_manifold_wave_small.ipynb
```


## Citation
To appear in AAAI-21; Meanwhile, read more on [[arXiv]](https://arxiv.org/abs/2006.15655)
```
@article{Mojgani_arxiv_2020,
	author = {Mojgani, Rambod and Balajewicz, Maciej},
	title={Physics-aware registration based auto-encoder for convection dominated {PDE}s},
	journal={arXiv preprint arXiv:2006.15655},
	archivePrefix = "arXiv",
	eprint = {2006.15655},
	year = 2020,
}
```
