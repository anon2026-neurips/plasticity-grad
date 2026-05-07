# Do Neural Networks Lose Plasticity in a Gradually Changing World?

### Abstract 
Continual learning has become a trending topic in machine learning research. Recent studies have discovered an interesting phenomenon called loss of plasticity, referring to neural networks gradually losing the ability to learn new tasks. However, existing plasticity research largely relies on contrived settings with abrupt task transitions, which often do not reflect real-world environments. In this paper, we propose to investigate a gradually changing environment, and we simulate this by input/output interpolation and task sampling. We perform theoretical and empirical analysis, showing that the loss of plasticity is an artifact of abrupt tasks changes in the environment and can be largely mitigated if the world changes gradually.

**Instructions for running experiments are inside each subfolder.**

### Analysis


We have conducted additional analysis showing relationship between interpolation granularities and four neural network properties.

<img width="880" height="331" alt="image" src="https://github.com/user-attachments/assets/9b0345d9-0c17-4c57-b313-6ef40fe9f7f4" />

**Maximum Hessian eigenvalue**: Previous work has shown that the maximum eigenvalue of the loss with respect to parameters can be interpreted as the sharpness of the loss landscape ([Dinh et al., 2017](https://proceedings.mlr.press/v70/dinh17b.html); [Lewandowski et al., 2024](https://arxiv.org/abs/2312.00246); [Lyle et al., 2024](https://proceedings.mlr.press/v202/lyle23b.html)). As shown in the figure, finer-grained transitions consistently produce lower maximum eigenvalues, indicating that the optimizer remains in a flatter, better-conditioned region of the loss landscape. This aligns with our theoretical analysis that under small task changes, the local smoothness is preserved. 

<img width="880" height="331" alt="granularity_hessian" src="https://github.com/user-attachments/assets/3999fba3-ee38-4e60-a64e-77d8c7246ea7" />

**Relative representation change**: 
We measure the change in hidden representations between consecutive task checkpoints on a fixed probe batch: 

$$
\mathrm{RelRepChange} = \mathbb{E}_{x}\left[ \frac{||h_{\mathrm{curr}}(x)-h_{\mathrm{prev}}(x)||}{||h_{\mathrm{prev}}(x)||} \right]
$$

Here, $h(x)$ is the hidden representation of input $x$ from the model.


As described by previous work: *“The average representation change is a proxy for plasticity, allowing us to see how much the behavior of the neural network has changed.([Lewandowski et al., 2025](https://openreview.net/forum?id=Hcb2cgPbMg))”*

As shown in the results, smaller interpolation steps better preserve the ability to update hidden representations between consecutive tasks, indicating that smoother task changes preserve the network’s ability to learn (plasticity).

<img width="880" height="331" alt="image" src="https://github.com/user-attachments/assets/84fde204-ac62-4d6f-8869-5a5abf7776f1" />

**Dormant unit fraction**: 
Following [Dohare et al., Nature; 2024](https://www.nature.com/articles/s41586-024-07711-7), a unit is dormant if it fires (post-ReLU activation > 0) on fewer than $\tau = 0.01\%$ of probe inputs. Formally, the fraction of \(\tau\)-dormant neurons is defined as:

$$\mathrm{DormantFraction} = \frac{ \sum_{l=1}^{L}\sum_{i=1}^{H^l} \mathbf{1}\!\left[ \frac{1}{N}\sum_{n=1}^{N}\mathbf{1}\!\left[a_{i,n}^l > 0\right] \le \tau \right] }{\sum_{l=1}^{L} H^l}$$, where $l$ indicates the layer, $H^l$ indicates the number of hidden units in layer $l$, $N$ is the number of probe inputs, $a_{i,n}^l$ denotes the post-ReLU activation of neuron $i$ in layer $l$ on probe input $n$, and $\tau$ is the dormancy threshold.


Finer-grained transitions consistently have a lower fraction of dormant neurons while also preserving plasticity. This aligns with previous studies showing that plasticity loss is associated with dormant neurons  ([Dohare et al., 2024](https://www.nature.com/articles/s41586-024-07711-7); [Sokar et al., 2023] (https://proceedings.mlr.press/v202/sokar23a.html)). 

<img width="880" height="331" alt="gran3_dormant_fraction" src="https://github.com/user-attachments/assets/b5a1cd9e-74a6-442b-9745-6791d9ecabb0" />

**Distance from initialization**: This property does not show a clear monotonic relationship with granularity. We hypothesize this is because our method does not impose any explicit regularization on the parameters (unlike L2-init or weight decay). 

<img width="880" height="331" alt="granularity_dist_init" src="https://github.com/user-attachments/assets/e82cd54a-0c17-402a-88b0-e3a5257d9a44" />
