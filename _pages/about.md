---
layout: about
title: 🇰🇷 About Me
permalink: /
subtitle: Bumjin Park
order : 1 
profile:
  align: right
  image: me.png
  address: >
    <p text-align:center;> AI Researcher </p>

news: true  # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

<p style="text-align: justify;">
Hi, my name is Bumjin Park, a Ph.D student in 
<a href="https://gsai.kaist.ac.kr/">the Graduate School of AI</a> in
<a href="https://www.kaist.ac.kr/en/">KAIST</a>.  
My research is focused on the understanding of mechanisms of Artificial Neural Networks motivated by the cognitive mechanism of humans. 
Feel free to contact for discussions on discovering the secrets of ANNs 🤗.
</p>
<!-- --------------------  Links -------------------- -->
<div style="display: grid;grid-template-columns: 3fr 2fr;border:1px solid #DDDDFF;border-radius:10px;padding:10px;align-items:center;">
<div style="line-height:2.0">
📨 <tag class="box-demo-link" style='color:#000000;background:#ffffff;border-radius: 10px;'>bumjin.research@gmail.com</tag> <br>
 📨 <tag class="box-demo-link" style='color:#000000;background:#ffffff;border-radius: 10px;'>bumjin@kaist.ac.kr</tag> 
 <li> 🧑🏻‍🎓 Ph.D Student at  <a class="box-demo-link" href="http://sail.kaist.ac.kr/" style="background:#0000AA"> SAIL.KAIST  </a>  </li>
<li> 🅖 <a class="box-demo-link" href="https://scholar.google.co.kr/citations?user=XzIXaxoAAAAJ&hl=ko" >Google Scholar</a>  </li>
<li> ⛳️ <a class="box-demo-link" href="">  CV (not available now) </a>  </li>
<li> 📝 <a class="box-demo-link" href="/share/full_paper_list"> Paper List </a>  </li>

<li> 🚀   <a class="box-demo-link" href="/main_projects/" style="background:#0000AA"> Project List </a>  </li>
</div>
<div>
  <img src="assets/common/me.png" width="100%" > 
</div>
</div>


<h1 style="font-family:Times New Roman;padding-top:2rem;"> 📌 Selected Papers   </h1>
<!-- -------------------- --------------------  Papers -------------------- -------------------- -->
<hr>
  <!--  ParchGrad: Controlled Internal Gradients for Reliable Saliency Map  -->
  <h3 class="demo-title"> ParchGrad: Controlled Internal Gradients for Reliable Saliency Map </h3>
  <div class="authors">Bumjin Park, Giyeong Jeon, and Jaesik Choi, 2023  
  </div>
  <div style="display: grid;grid-template-columns: 1fr 1fr;border:1px solid #DDDDFF;border-radius:10px;padding:10px;align-items:center;">
    <div>
    <ul>
    <li> <strong> Motivation </strong> : Gradients are shattered, and more refined internal signals can improve the saliency map. </li> 
    <li> <strong> Problem </strong> : Comparison of internal gradients in a convolutional layer and make a measure properly compare gradient signals.  </li>
    <li> <strong> Method </strong> : We propose $\gamma$-dominance based on the random variable formulation of gradients. </li>
    <li> <strong> Contribution </strong> : We propose variance conservation and class-wise channel selection to safely prune internal gradient signals. </li>
    <li> <strong> Keywords </strong> : saliency map, channel pruning </li>
    </ul>
      </div>
    <div> {% include figure.html path="https://drive.google.com/uc?export=view&id=1kJwgA-XPdP0k3cd60R8H7jdqtgLbLJ44" class="img-demo" zoomable=true %} 
    </div>
  </div>
  <center>
  <div class="demolink" style='padding-bottom:1rem;padding-top:0.5rem;'>
    <a class="box-demo-link" href="" style="background:#AA00AA" >Pre-print</a> | 
      <a class="box-demo-link" href="/main_papers/parchgrad" style="background:#0005AA;" >Project</a> |
      <a class="box-demo-link" href="https://github.com/fxnnxc/Parchgrad" style="background:#000000;">Code</a> |
    <a class="tooltip-wrap">
      <span class="tooltip-span"> Abstract </span>
      <div class="tooltip-content">
      <strong> Abstract </strong> <br>
      Explaining the decision of neural networks is crucial to ensure their reliability in various image tasks. Among several input attribution methods, the saliency map explains the decision with the propagated gradients on pixels. One of the fundamental problems of the saliency map is gradient shattering which leaves noise on pixels and hinders the interpretation of attribution maps. To provide a clear explanation, controlling the internal gradient signals is essential. However, to the best of our knowledge, there is no work on modifying the internal gradient signals so that the saliency map provides reliable explanations. To initialize the control of internal gradient signals in the saliency map, we provide theoretical analysis on the controllable modification of gradients in multiple convolutional channels and propose ParchGrad, which prunes channels by partitioning multiple channels into two groups. In addition, we propose a class-wise selection method based on statistical tests to select which channels to magnify or reduce the impact of gradients. Experiments on the ImageNet1k dataset with several types of convolutional networks show that controlling the gradients with ParchGrad provides more faithful and sparse explanations.  
      </div>
    </a> 
  </div> 
    </center>

<!-- -------------------- --------------------  Papers -------------------- -------------------- -->
    




<br style='margin-bottom:50px;'>

<!-- <a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> | 

<a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> |  -->


<!-- <iframe src="/assets/html/sail_research.drawio.html" width="100%" height="500px"></iframe> -->

<hr>



