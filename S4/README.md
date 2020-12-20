# Handwritten Digit Recognition with a Convolutional Neural Network Trained on the MNIST Dataset: a minimalistic approach 

1. Goal: **99.4%** testing accuracy
2. Constraints
    1. Less than **20,000** parameters
    2. Less than **20** epochs
    3. No fully connected layers
    
4. Layers and blocks
    1. All feature extraction blocks (`conv1` ~ `conv6`) included the following layers:
        1. 3x3 convolutional layer with `padding=1` and `stride=1`
        2. ReLU layer
        3. Batch Normalization layer
        4. Dropout layer with `p=0.1`
    2. All maximum pooling layers had `kernel_size=2` and `stride=2`  
    3. All 1x1 convolution layers (`tran` and `conv7`) had padding=1 and stride=1
    4. Used the global average pooling layer instead of a fully connected layer
    5. Output went through `log_softmax()`, which when used with `NLLLoss()` has the effect of using a cross entropy loss function

5. Model structure and dimensions

| Layer / Block | Input        | Kernel            | Output       | Receptive Field
| ------------- | ------------ | ----------------- | ------------ | ---------------
| conv1         | 28 x 28 x 01 | 03 x 03 x 01 x 08 | 28 x 28 x 08 | 03
| conv2         | 28 x 28 x 08 | 03 x 03 x 08 x 16 | 28 x 28 x 16 | 05
| pool1         | 28 x 28 x 16 | 02 x 02 x 01 x 16 | 14 x 14 x 16 | 10 
| trans         | 14 x 14 x 16 | 01 x 01 x 16 x 08 | 16 x 16 x 08 | 10
| conv3         | 16 x 16 x 08 | 03 x 03 x 08 x 16 | 16 x 16 x 16 | 12
| conv4         | 16 x 16 x 16 | 03 x 03 x 16 x 16 | 16 x 16 x 16 | 14
| pool2         | 16 x 16 x 16 | 02 x 02 x 01 x 16 | 08 x 08 x 16 | 28
| conv5         | 08 x 08 x 16 | 03 x 03 x 16 x 32 | 08 x 08 x 32 | 30
| conv6         | 08 x 08 x 32 | 03 x 03 x 32 x 32 | 08 x 08 x 32 | 32
| conv7         | 08 x 08 x 32 | 01 x 01 x 32 x 10 | 10 x 10 x 10 | 34
| gap           | 10 x 10 x 10 | 10 x 10 x 01 x 10 | 01 x 01 x 10 | 34

6. Total parameters used: **19,330**

7. Best test accuracy: **99.36%** (consistently fluctuated between 94.00% and 95.00% after adjusting the kernel size to 7x7 for the global average pooling layer, but this adjustment was made after the submission deadline)
