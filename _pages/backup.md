
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
    <div> {% include figure.html path="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21835&authkey=%21ABDn33HR1njl1fM&width=1035&height=565" class="img-demo" zoomable=true %} 
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