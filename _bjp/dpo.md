---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-01-01
featured: true
title: 'Try to understand the mathematical derivations of direct preference optimization (DPO)'
description: 'DPO is a recently proposed algorithm to learn an instruction tuning. We discuss on the mathematical formulation of DPO'
---


DPO <d-cite key="rafailov2023direct"/>


<table style="display:block; grid-column:middle; width:110%;margin-left:-40px;padding-right:50px;" markdown="1">
<thead>
    <tr>
        <th> Description </th>
        <th> Equation </th>
    </tr>
</thead>
<tbody>
<!-- Equation 1 -->
<tr>
    <td>
        preference distribution $p^*$ for BT model
    </td>
    <td>
        $$ 
        \begin{equation}
        p^*(y_1 \succ y_2 | x ) = 
        \frac{
            \exp (r^* (x,y_1))
            }
            {
                \exp(r^* (x,y_1)) +\exp(r^* (x,y_2))
            }
        \end{equation}
        $$  
    </td>
</tr>
<!-- Equation 2 -->
<tr>
    <td>
        binary classification problem framed for BT model<d-footnote> $\sigma$ is a sigmoid function.  </d-footnote>
    </td>
    <td>
        $$ 
        \begin{equation}
        \mathcal{L}_R(r_\phi, \mathcal{D}) 
        = 
        - \mathbb{E}_{(x,y_w, y_l) \sim \mathcal{D}}
        [
            \log \sigma(r_\phi(x, y_w) - r_\phi(x, y_l))
        ]
        \end{equation}
        $$  
    </td>
</tr>
<!-- Equation 3 -->
<tr>
    <td>
        RL fine-tuning for GPT 
    </td>
    <td>
        $$ 
        \begin{equation}
        \max_{\pi_\theta} \mathbb{E}_{x\sim \mathcal{D}, y\sim \pi_\theta (y|x)}[r_\pi(x,y)] 
        - \beta \mathbb{D}_\mathrm{KL}[\pi_\theta(y|x) \Vert \pi_\mathrm{ref}(y|x)]
        \end{equation}
        $$  
    </td>
</tr>

<!-- Equation 4 -->
<tr>
    <td>
        The form of the optimal solution of Equation 3
    </td>
    <td>
        $$ 
        \begin{equation}
        \pi_r(y|x) = \frac{1}{Z} \pi_\mathrm{ref}(y|x) \exp \Big( \frac{1}{\beta} r(x,y) \Big)
        \end{equation}
        $$  
    </td>
</tr>

<!-- Equation 5 -->
<tr>
    <td>
        reward function obtained from Equation 4. 
    </td>
    <td>
        $$ 
        \begin{equation}
        r(x,y) = \beta \log \frac{\pi_r(y|x)}{ \pi_\mathrm{ref}(y|x)} + \beta \log Z(x)
        \end{equation}
        $$  
    </td>
</tr>

<!-- Equation 6 -->
<tr>
    <td>
        The preference probability expressed in terms of the policies.
    </td>
    <td>
        $$ 
        \begin{equation}
        p^*(y_1 \succ y_2 | x) = \frac{1}
                    {
                        1+ \exp \Big( \beta \log \frac{\pi^*(y_2|x)}{\pi_\mathrm{ref})y_2} 
                        - \beta \log \frac{\pi^* (y|1)}{\pi_\mathrm{ref}(y_1|x)} \Big)
                    }
        \end{equation}
        $$  
    </td>
</tr>

<!-- Equation 7 -->
<tr>
    <td>
        DPO maximum likelihood objective 
    </td>
    <td>
        $$ 
        \begin{equation}
        \mathcal{L}_\mathrm{DPO}(\pi_\theta ; \pi_\mathrm{ref}) = 
        - \mathbb{E}_{(x,y_w, y_l)\sim \mathcal{D}} \Big[ 
            \log \sigma 
                \Big( 
                    \beta \log \frac{\pi_\theta (y_w|x)}{\pi_\mathrm{ref}(y_w | x)}
                    -
                    \beta \log \frac{\pi_\theta (y_l|x)}{\pi_\mathrm{ref}(y_l | x)}
                \Big)
            \Big]
        \end{equation}
        $$  
    </td>
</tr>
</tbody>
</table>



## Logic


## Derivations 

## Gradient Analysis 


## Theorem 