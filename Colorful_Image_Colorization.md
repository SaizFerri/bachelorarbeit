## Colorful Image Colorization

### Introduction

Task is to produce a plausible colorization that could potentially fool a human observer, rather than recreate the ground truth. The system uses the *Lab* color space and given the *L* channel predicts both color channels *ab*.

Other work have trained CNNs to predict color in large datasets. The results tend to look desaturated (<small>Cheng, Z., Yang, Q., Sheng, B.: Deep colorization. In: Proceedings of the IEEE International Conference on Computer Vision. (2015) 415–423 & Dahl, R.: Automatic colorization. In: http://tinyclouds.org/colorize/. (2016)</small>). One explanation is that the loss function used encourages conservative predictions. This losses are inherited from standard regression problems, where the goal is to minimize the euclidean error between an estimate and the ground truth.

The paper instead uses classification. Color prediction is multimodal, thus same objects can have many colorization options. To appropriately model the multimodal nature of the problem, the authors predict a distribution of possible colors for each pixel. At training time the loss is re-weighted to emphasize rare colors.

### Approach

**2.1 Objective Function**

Given an input lightness channel $ X \in \mathbb{R}^{H \times W \times 1} $, the objective is to learn a mapping $\hat{Y} = \mathcal{F}(X)$ to the two associated color channels $ Y \in \mathbb{R}^{H \times W \times 2} $, where H, W are image dimensions. We perform this task in CIE *Lab* color space. Because distances in this space, model perceptual distance, a natural objective function, as used in [1,2], is the Euclidean loss L2 between predicted and ground truth colors:

$L_2(\hat{Y}, Y) = \frac{1}{2} \sum_{h,w}\parallel Y_{h,w} - \hat{Y}_{h,w} \parallel_2^2$

However this loss is not robust to the inherent ambiguity and multimodal nature of the colorization problem. If an object can take on a set of distinct ab values, the optimal solution to the Euclidean loss will be the mean of the set. In color prediction, this averaging effect favors grayish, desaturated results. Additionally, if the set of plausible colorizations is non-convex, the solution will in fact be out of the set, giving implausible results.

Instead the problem is treated as a multinomial classification. The *ab* output space is quantized into bins with grid size 10 with a result of $Q = 313$ bins. For a given input $X$ a mapping $\hat{Z} = \mathcal{G}(X)$ is learned to a probability distribution over possible colors $\hat{Z} \in [0, 1]^{H \times W \times Q}$, where $\mathcal{Q}$ is the number of quantized *ab* values.





**2.3 Class probabilities to Point Estimates**

*annealed-mean*: Kirkpatrick, S., Vecchi, M.P., et al.: Optimization by simmulated annealing. science 220(4598) (1983) 671–680