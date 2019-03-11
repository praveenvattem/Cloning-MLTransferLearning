# **Behavioral Cloning** 

## Writeup Template
---
**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.ipynb containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results
* drive.py for converting the saved images into video file.
* run1 folder output images while car running in autonomous mode
* run1.mp4 to see the autonomous mode car driving

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```python drive.py model.h5
```
#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

My model consists of a convolution neural network with 3x3 filter sizes and depths between 32 and 128 (model.ipynb lines 9th block ) 

The model includes RELU layers to introduce nonlinearity, and the data is normalized in the model using a Keras lambda layer.

#### 2. Attempts to reduce overfitting in the model

The model contains dropout layers in order to reduce overfitting (model.ipynb lines 9th block ) . 

The model was trained and validated on different data sets to ensure that the model was not overfitting The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually(model.ipynb lines 9th block ) .

#### 4. Appropriate training data
Downloaded the sample data from the link provided saved in data folder.read the relative path of images.
I used a combination of center lane driving, recovering from the left and right sides of the road by taken three camera images into one image calculating the angle position as error from center.
 

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy for deriving a model architecture was to ...
In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. 

To combat the overfitting, I modified the model and added few parameter changes in dense , middle layers structure etc.

The final step was to run the simulator to see how well the car was driving around track one. the learning rate was needed to improve model , car felloff at the curvature . so modified the learning rate and epoches.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

The final model architecture (model.ipynb lines 9th block ) consisted of a convolution neural network with the following layers and layer sizes ...

Here is a visualization of the architecture (note: visualizing the architecture is optional according to the project rubric)

_Layer (type)                 Output Shape              Param #   
=================================================================
lambda_1 (Lambda)            (None, 160, 320, 3)       0         
_________________________________________________________________
cropping2d_1 (Cropping2D)    (None, 90, 320, 3)        0         
_________________________________________________________________
conv2d_1 (Conv2D)            (None, 43, 158, 24)       1824      
_________________________________________________________________
conv2d_2 (Conv2D)            (None, 20, 77, 36)        21636     
_________________________________________________________________
conv2d_3 (Conv2D)            (None, 8, 37, 48)         43248     
_________________________________________________________________
conv2d_4 (Conv2D)            (None, 6, 35, 64)         27712     
_________________________________________________________________
conv2d_5 (Conv2D)            (None, 4, 33, 64)         36928     
_________________________________________________________________
flatten_1 (Flatten)          (None, 8448)              0         
_________________________________________________________________
dropout_1 (Dropout)          (None, 8448)              0         
_________________________________________________________________
dense_1 (Dense)              (None, 100)               844900    
_________________________________________________________________
dropout_2 (Dropout)          (None, 100)               0         
_________________________________________________________________
dense_2 (Dense)              (None, 50)                5050      
_________________________________________________________________
dense_3 (Dense)              (None, 10)                510       
_________________________________________________________________
dense_4 (Dense)              (None, 1)                 11        
=================================================================


#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

After the collection process, I had 24108 data images. I then preprocessed this data by combining center, left, right images into imagepath & measurement parameter.

Split the total data points into train and validation samples 
Train samples: 19286
Validation samples: 4822 
For training the data i used generator , the image file size is very large. 
while generating yields i flipped, trimmed and crop the image to get the useful information. 

The training set= train_generator , test_set = Validation_generator

I trained the model using Cuda 10 GPU for effective training of the model.
I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 4 as evidenced by ... I used an adam optimizer so that manually training the learning rate wasn't necessary.
The mse for the model is as below
Epoch 1/4
603/602 [==============================] - 1063s 2s/step - loss: 0.0195 - val_loss: 0.0156
Epoch 2/4
603/602 [==============================] - 997s 2s/step - loss: 0.0162 - val_loss: 0.0150
Epoch 3/4
603/602 [==============================] - 874s 1s/step - loss: 0.0148 - val_loss: 0.0137
Epoch 4/4
603/602 [==============================] - 878s 1s/step - loss: 0.0141 - val_loss: 0.0127


