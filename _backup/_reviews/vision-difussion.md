


Score-based Generative Model 

Train a vector field. 

Low dimensional manifold 

Gaussian noise of various magnitude 

* Annealed version of Langevian dynamics 
    * noise sigma 의 텀을 다양하게 줘서 커버 가능하게 샘플링 하자. 



모든 노이즈 레벨을 고려하자. 

$$
\frac{1}{2} \mathbb{E}_{p_{data}} [\Vert s_\theta (x) - \nabla_x \log p_{data}(x) \Vert_2^2]
$$

* Denoised Score Matching 
    * Score를 noise를 활용해서 추정한다. 
* Sliced Score Matching 

* Manifold Hypothesis 
    * 실제 데이터들은 High dimensional 하게 있기만 Low dimensional 에 있는 Collapsed Samples
* NCSN 
* $q_\theta(\tilde{x}|x) = \mathcal{N}(\tilde{x}| x, \sigma^2 I)$
* sigma 에 의존적이지 않은 loss를 만들기 위해서 $\lambda = \sigma^2$.