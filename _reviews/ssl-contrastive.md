


Embedding matrix $K\in \mathbb{R}^{M\times N}$ where $M$ is embedding size and $N$ and number of samples. 


### Contrastive criteria

Penalizing the similarity between different pairs of images. 

$$
L_c = || K^\top K - \mathrm{diag}(K^\top K)||_F^2
$$


### Non-contrastive criteria

Penalizing the off-diagonal terms of the covariance matrix of the embeddings.

$$
L_{nc} = ||  KK^\top - \mathrm{diag}( KK^\top)||_F^2
$$

