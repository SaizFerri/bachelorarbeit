# Prototype

## Dataset and Preprocessing

1. Use CIFAR-100 as the dataset
2. Use transforms to preprocess the data
3. Use skimage to convert from RGB to Lab and viceversa

## Model

1. U-Net model with skip connections
2. **Maybe** VGG-16 model

## Loss and optimizer

* L2 regression loss for performance purposes
* Cross-entropy loss for classification wiht bins

* Adam optimizer with LearningRate decay when loss plateus