---
layout: page
title: 🌈 Edge Oriented Neural Implicit Representation [Korean]
description: Neural implicit representation by separation of edge and color
img: assets/img/proj8/structure.png
importance: 2
category: work
---

* 🤗 Coworkers  : Wonjoon and Dahee (SAIL)
* ⏱ Data : 2021.11
* 🔖 Keywords : Computer Vision, Neural Implicit Representation 
* 🎯 Output : NIR Algorithm and Gradient Algorithm

# 1. Neural Implicit Representation

NIR은 네트워크가 이미지의 픽셀값을 단순히 위치만으로 모두 기억하는 모델이며, 만일 실제 이미지 용량 보다 네트워크 사이즈가 작다면 데이터를 압축하는 효과가 있다. 이미지에는 수많은 정보가 불필요하게 반복될 수 있으나 NIR을 사용하면 파라미터에 그 정보가 함축적으로 저장된다. 예를 들어서 단순히 빨간색 그림 🟥 이라면 이미지 파일은 RGB를 너비와 높이 크기만큼 기억해야 하지만, 사실 우리는 단순히 (255,0,0) 이라는 값만 있으면 된다. 비슷하게 간단한 그림이라면 해당 위치에 대한 픽셀을 기억하는 것이 더욱 효율적이다. **이미지상에 특정한 패턴이 있고, 그 패턴이 사람이 계산하긴 어렵지만 인공지능으로는 할 수 있는 경우가 존재한다.** 신경망을 이용하면 좀더 복잡한 그림일지라도 그 정보를 효율적으로 함축할 수 있다. 이에 사용되는 대표적인 모델은 SIREN으로 이미지를 시그널로 생각해서 모델링 하는 방식이다. 그림은 실제 이미지와 SIREN으로 복원된 이미지를 보여준다. 두 눈으로 보기에 큰 차이가 없어 보이나, SIREN의 경우 이미지의 Gradient를 제대로 표현하지 못한다. 이는 Gradient가 Delta Function과 비슷한 반면, SIREN은 부드러운 주기함수를 이용하기 때문이다.


<center>
<div class="row mt-3">
        {% include figure.html path="assets/img/proj8/problem.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p> Figure 1. SIREN model fitting on the Dataset </p>
</center>


프로젝트의 목표는 단순히 Pixel을 복원하는 것이 아니라, Gradient를 우선적으로 복원하고 거기에 색깔을 입히는 Edge-Oriented Nueral Implicit Representation (EoREN) 방식을 제안한다.  추가적으로 Gradient 를 정확하게 모델링하기 위해서, Sobel Filter로부터 추가되는 가중치를 줄이는 알고리즘을 제안한다. 


# 2. Image Gradient


이미지의 픽셀에 대해서 RGB 값이 변화하는 정도를 Gradient 로 표현할 수 있다. Gradient 는 색깔의 변화정도를 나타내므로, 단순한 이미지는 0에 가까운 값을 가지고, 복잡한 이미지는 Gradient 값이 0 이 아닌 부분이 많다. 

첫 번째 Contribution은 Sobel Filter 를 이용하여 Gradient를 구할 때 나타나는 Magnitude 에 대한 Normalization과 Image Size에 대해서 Normalize 하여 Gradient Target을 조정하는 방식이다. 
Gradient with Magnitude Adjustment는 주어진 이미지와 필터에 대해서 Gradient를 구하는 알고리즘이다. NIR로 학습할 때, (x,y)픽셀 위치에 대한 타겟 Gradient로 사용된다. 

<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/gma_result.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 2. Pixel 에 대해서 기존 Sobel Filter로 인해서 생기는 Gradient 시각화. Sobel 필터를 적용할 경우, Weight들이 존재하며, 해당 값들은 Normalize가 되지 않기 때문에 이미지의 Gradient가 증폭된다. 이는 Gradient 를 타겟으로 하는 경우 문제가 된다. (오른쪽) 실제 이미지의 Gradient는 오른쪽과 같이 Weight에 대해서 Normalize 를 해서 나타난다. </p>
</center>


<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/gma.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 3. Gradient with Magnitude Adjusting 알고리즘. 필터의 Weight에 대해서 Normalize 를 진행하고, 이미지 사이즈에 대해서도 Normalize 를 진행한다. 이미지의 사이즈에 대해서도 하는 이유는 (x,y) 좌표를 0에서 1 사이의 실수값으로 가정하기 때문이다.  </p>
</center>






# 3.  Edge Oriented Neural Implicit Representation 


이미지의 채널별로 색상이 변하는 정도를 Gradient로 표현하게 된다면, RGB 채널에 대해서 독립적으로 고려할 수 있다. 기존 Neural Implicit Representation 은 RGB 값을 모델링하는 방식으로 연구하였고, Gray Scale Image 에 대해서만 Gradient 를 모델링하였다. 이 프로젝트에서는 RGB Channel Image에 대해서 Gradient 를 모델링 하고, Color값에 해당하는 정도를 이후에 모델링하여, 두 부분을 효율적으로 나누는 방식을 연구하였다. 


<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/structure.png" class="img-fluid-100 rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 4. EoREN 의 구조.  </p>
</center>




# 4. Experiment

SIREN과 같은 모델로 Gradient Fitting을 완료하고 나면, 이미지의 Edge를 더욱 잘 따는 모델을 가지게 된다. 실제 EorREN이 잘 복원한 이미지와 그렇지 않은 이미지를 보면, EoREN 모델이 이미지의 Edge를 더욱 선명하게 복원한다. 


# 5. Results 

<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/siren_eoren.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 4. EoREN 의 구조.  </p>
</center>



<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/mnist_recon.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 4. EoREN 의 구조.  </p>
</center>


<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/mnist_sample.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 4. EoREN 의 구조.  </p>
</center>



성능테스트를 위해서 MNIST 데이터에 대하여 SIREN 으로 Pixel 을 복원한 경우와  EoREN으로 학습한 경우를 비교해 보자. 동일한 Epoch에 대해서 학습할 경우, EoREN은 SIREN보다 더욱 높은 복원 성능을 보였는데, 이는 MNIST 데이터셋이 단순하기 때문이다. Edge에 대한 정보가 단순하게 있으며, 일부 픽셀 위치에 대해서 Frequnecy 가 높다. 이미지 입장에서 보면 
* Pixel : Constant Function 의 형태를 이룬다. 
* Gradient : Delta Function 의 형태를 이룬다. 
따라서 Gradient로 Fitting 했을 때 성능이 더 높았던 것이다. 
결국 Gradient Fitting은 이미지의 특징점들이 상대적으로 적은 경우에 유용하게 사용될 수 있다. 

두 이미지를 합성하는 경우, EoREN 으로 Edge를 더욱 잘 따는 것을 볼 수 있다. 반면에 Pixel Fitting은 이미지의 전반적인 Color에 대하여 평균적인 값을 예측하므로 상대적으로 Smooth 한 그림이 나타난다. 



<center>
<div class="row justify-content-sm-center">
        {% include figure.html path="assets/img/proj8/composition.png" class="img-fluid rounded z-depth-2 max-width-3" zoomable=true %}
</div>
<p align="left"> Figure 4. EoREN 의 구조.  </p>
</center>


# 5. Conclusion 

Neural Implicit Representation 은 이미지에서 픽셀 위치에 대응 하는 값을 배우는 딥러닝 기반 연구이다. 이미지를 기억하기 위해서 픽셀을 외울 수도 있지만 (Pixel Fitting), 이미지의 그래디언트 (Gradient Fitting) 을 할 수도 있다. 픽셀 피팅과 그래디언트 피팅은 모두 이미지를 기억하는데 어느정도 효율적이다. 그래디언트 피팅은 이미지에서 엣지에 해당하는 부분을 더욱 선명하게 학습하는 장점이 있다. 그러나 잘못된 그래디언트는 전체적인 이미지에 영향을 줄 수 있으므로 좀더 세밀한 주의가 필요하다. 두 개 중에 어떤 것을 사용해야 하는지에 대해서는 아직까지 결론이 나지 않았으므로 앞으로 연구 해볼만한 주제이다. 

