## <i>ColorUNet</i>: A convolutional classification approach to colorization

URL: [https://arxiv.org/pdf/1811.03120.pdf](https://arxiv.org/pdf/1811.03120.pdf)

### Introduction

* Image to Image translation problem
* Classification problem  to gain flexibility in the prediction process and output more colorful images
* YUV Colorspace
* Architechture designed from the scratch to ensure modularity, full control of the design and to keep the overall complexity reasonable

### 1. Related work

Work with human interacction:

* M. He, J. Liao, L. Yuan, and P. V. Sander. Neural color transfer between images. CoRR, abs/1710.00756, 2017
* A. Levin, D. Lischinski, and Y. Weiss. Colorization using optimization. In ACM Transactions on Graphics (ToG), volume 23, pages 689–694. ACM, 2004.

Automated methods with L2 regression:

* Z. Cheng, Q. Yang, and B. Sheng. Deep colorization. In Proceedings of the IEEE International Conference on Computer Vision, pages 415–423, 2015.
* R. Dahl. Automatic colorization. http://tinyclouds.org/colorize/, 2016.

Problems with regression:

* penalizes predictions that fall overall too far from the ground truth. As a consequence, the models trained following such methods usually tend to be very conservative and to give desaturated, pale results.

Paper bases on:

* R. Zhang, P. Isola, and A. A. Efros. Colorful image colorization. In European Conference on Computer Vision, pages 649–666. Springer, 2016.

**Problem** with this method is the size of the network and the time it takes to train it. The size of the training set is prohibitive, with several millions of images. <i>"The reason behind this complexity is that in order to encode meaningful features that help to colorize the image, the receptive field has to be large."</i> <br>
**Solution**: O. Ronneberger, P. Fischer, and T. Brox. U-net: Convolutional networks for biomedical image segmentation. CoRR, abs/1505.04597, 2015. <i>"**connections between hidden layers** of a bottleneck neural network could enhance the performance greatly, by injecting locational information in the upsampling process, and improving the gradient flow"</i>

### 2. Methods

**2.1 Colorization as a classification problem and using a (weighted) cross entropy loss**

* Split colormap into equal sized bins
* <i>n</i> most frequent color bins as learned on a large dataset beforehand
* if a color doesn't fall within one of the <i>n</i> bins, it is assigned to the closest available (this simplification leads to a rather faint degradation of the images)
* To boost the possibility of a rare color being predicted:
  * *rebalancing* to give larger weights to rare colors in the loss function:   $\omega_{p} \propto \begin{pmatrix}(1-\lambda) * \widehat{P}(b) + \frac{\lambda}{n}\end{pmatrix}^{-1}$ , where $b \in \mathbb{R}^n$ is the closest bin, $\omega_{p}$ is a given weight, $\widehat{P}(b)$ is the estimated probability of the bin (computed prior to our model’s training) and $\lambda \in [0, 1]$ a tuning parameter (the closer to 1, the less we take the bin’s frequency into account)
  * *annealed-mean* to output predictions y from the probability distribution Z over our n bins to the full original color space
    TODO: softmax like function

**2.2 Neural architecture: ColorUNet**

* Flat convolutional neuronal network with 3 layers gave poor performance because the receptive field was not large enough to capture meaningful general shapes.
* Bottleneck architecture without skip connections was very unstable and slow to train, which is easily explained by the depth of the architecture.
* **Final architecture: Bottleneck architecture with skip connections**

### 3. Dataset and features

**3.1 Datasets**

* Subset of the SUN and ImageNet datasets.
* Nature landscape categories
* Final dataset 13,469 images, validation set 2,889 images
* Size 256x256 with downsampling if necessary with the Lanczos method.

**3.2 Data argumentation**

* Flipping the image along the horizontal axis
* Adding noise to the image (with different intensities)
* Selecting random crops of the image

### Results

**4.1 Evaluation and parameter tuning**

* No relevant metric to meassure the performance of the network. Classification loss is only useful for training. L2 loss is not relevant as discussed in section 1. **Best** is to rely on human evaluation for the actual predictions
* Final number of classes for the color discretizer is $n = 32$
* Number of layers around 32 to have a computationally reasonable model
* Learning rate adapted to loss and Adam optimizer
* Trained in 2 steps with a learning rate decay between both
* Batch size 64
* Comparison between state-of-the-art model to compare results and sanity check of the model

**4.2. Colorizing images**





