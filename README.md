# Image classification of Misaligned and Faulty Fan Wires in the Device under Test
### Objectives
1. To design the CNN-based deep neural network (DNN) to infer the presence of misaligned and faulty fan wire connections in the device under test
2. To improve the testing performance by fine-tuning the pre-trained weights of convolution layers for the target dataset
3. To develop the image classification application with the trained model to infer the real-time status of fan wire connections in the webcam images for automated optical insepction (AOI) applications

### Fan Wires Images   
**PASS**   
![](https://i.imgur.com/nrsWnCX.png)   
**FAIL_CONN** (Faulty Connection)   
![](https://i.imgur.com/2iKqc6k.png)   
**FAIL_PT**   
![](https://i.imgur.com/UNsUwdp.png)   
**Note**: The wires did not pass through the white plastic socket      

## Dataset and Preprocessing   
1. Dataset: 1541 images per class (FAIL_CONN, FAIL_PT, PASS). The images are captured at different distances from the device and also at different angles to induce more variations in the training (80%) and validation (20%) datasets  
2. Convert the color space of the image from BGR to RGB
3. Resize the image to 400 x 400. The default image size of 224 x 224 is not sufficient to classify the FAIL_PT due to its smaller dimension.
4. Subtract the per-channel mean of the imagenet dataset (RGB:[123.68, 116.779, 103.939]) from the resized image   

### Preprocessed Images   
**PASS**   
![](https://i.imgur.com/16O7MhV.png)   

**FAIL_CONN**   
![](https://i.imgur.com/VHyAe0h.png)   

**FAIL_PT**   
![](https://i.imgur.com/Acx7qI7.png)


## Training
1. Refer to this [link](https://github.com/ramesh-dev-code/misaligned-heat-sink/blob/main/README.md#training-platform-setup) for the training platform setup   
2. Unfreeze all the convolution layers in the VGG-16 model. This fine-tunes the pre-trained weights of the convolution layers by removing the unneccessary variations so as to fit for the target dataset. This step significantly improves the performance on the testing images compared to the fixed weights trained on the imagenet dataset.   
3. The [previous experiments](https://hackmd.io/CqLn94oZR1WqxCK99paukw?view#Image-Classification-of-Faulty-Alignment-of-Fan-Wires) on the target dataset suggest the fully-connected network with **two 256-node** hidden layers and a **3-node** output layer with the **softmax classifier**   
4. Training Epochs: 30, Execution Time: 34.6 m, Trained Model: best_model_02-02-2021_10:25:08.h5   

## Training and Validation Performance   
![](https://i.imgur.com/xDi5J7M.png)   
![](https://i.imgur.com/O1vR9YR.png)   

## Prediction    
1. Capture the webcam image   
2. Change the color space from BGR into RGB   
3. Resize the image into the dimensions of 400 x 400   
4. Subtract the per-channel mean of the imagenet dataset from the resized image   
5. Predict the output class of the image with the trained model   

```
python3 predict.py
```
### Sample Outputs   
**FAIL_CONN**   
![](https://i.imgur.com/bomQ8Fo.png)   

**FAIL_PT**   
![](https://i.imgur.com/FCVtecP.png)   

**PASS**   
![](https://i.imgur.com/Pjns41L.png)   


