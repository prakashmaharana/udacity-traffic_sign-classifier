# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./Exploratory_visualization.png "Visualization"
[image4]: ./images/image001.jpg "Traffic Sign 1"
[image5]: ./images/image002.jpg "Traffic Sign 2"
[image6]: ./images/image003.jpg "Traffic Sign 3"
[image7]: ./images/image004.jpg "Traffic Sign 4"
[image8]: ./images/image005.jpg "Traffic Sign 5"


### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data ...

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

((x - np.mean(x),axis=0)/(np.std(x),axis=0)
Normalize image data z = (x-mean(x))/std(x)

z-score of 0 is at the mean of the distribution and a z-score of 2.0 or beyond is in the tails of the distribution.  
A negative z–score means that the original score was below the mean. 
As the sample size becomes large, approximately half the z-scores should be negative and half of the 
z-scores should be positive.

[here](https://en.wikipedia.org/wiki/Grayscale#Converting_color_to_grayscale)
Y' = 0.299 R + 0.587 G + 0.114 B 
np.dot(rgb[...,:3], [0.299, 0.587, 0.114])


The difference between the original data set and the augmented data set is the following ... 


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 				|
| Input         		| 14x14x6 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, valid padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16 				|
| Flatten	      	| Input = 5x5x16. Output = 400 				|
| Fully connected		| Input = 400 Output = 120     dropout = 0.6    									|
| RELU					|												|
| Fully connected		| Input = 120 Output = 84     dropout = 0.6    									|									|
| RELU					|												|
|	Fully connected		| Input = 84 Output = 43     
|						|												|
 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an ....

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.938
* validation set accuracy of 0.938
* test set accuracy of 0.600

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?

CNN and used http://yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf

* What were some problems with the initial architecture?

Initial architecture had accuracy around 87 and even with changing epochs,batch size or learning rate validation accuracy was not improving.When i converted colored images to gray i tried to apply histogram equalization to check if that would improve accuracy but at the end regularization using drop out of 60% helped. 

* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.

I added drop out with 0.6 keep i.e 60% is kept and rest is dropped ,accuracy jumped to 93.8

* Which parameters were tuned? How were they adjusted and why?

Epochs = 10
Batch size = 128
learing rate = 0.001

Epochs indicate total number of times data is run through given network
We need to tweak this parameters so that cost is between underfitting to overfitting
i.e increase till validation accuracy decreases and training accuracy increases avoid overfitting
If validation data is not provided take 20% of training data as good rule of thumb

batch size indicates total training data for a given batch.Increase for better results.

Dropout for avoiding overfitting.

* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

Without dropout layer Validation accuracy was improving with every epoch than it started declining.When i added drop out layer to CNN i started with keep = 0.5 as mentioned in lecture but after epoch 8 it started declining but accuracy was 90% which was better when i didn't had 

If a well known architecture was chosen:
* What architecture was chosen?

CNN and used this document as reference http://yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf

For optimizer adam optimizer was used instead of classical stochastic gradient discent because Stochastic gradient descent maintains a single learning rate for all weight updates and the learning rate does not change during training.

In Adam optimizer learning rate is maintained for each network weight and separately adapted as learning unfolds.Also adam was easier to implement .

* Why did you believe it would be relevant to the traffic sign application?

Since this is image and CNN would work better.

* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
 
 Final model accuracy was 93.8 % after dropout was added with keep = 0.6
 
 Training...

EPOCH 1 ...
Validation Accuracy = 0.685

EPOCH 2 ...
Validation Accuracy = 0.812

EPOCH 3 ...
Validation Accuracy = 0.860

EPOCH 4 ...
Validation Accuracy = 0.895

EPOCH 5 ...
Validation Accuracy = 0.900

EPOCH 6 ...
Validation Accuracy = 0.927

EPOCH 7 ...
Validation Accuracy = 0.934

EPOCH 8 ...
Validation Accuracy = 0.927

EPOCH 9 ...
Validation Accuracy = 0.944

EPOCH 10 ...
Validation Accuracy = 0.938

Model saved

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6] 
![alt text][image7] ![alt text][image8]

The first image might be difficult to classify because ...

INFO:tensorflow:Restoring parameters from ./lenet
New Image 1
New Image Accuracy = 1.000

INFO:tensorflow:Restoring parameters from ./lenet
New Image 2
New Image Accuracy = 0.500

INFO:tensorflow:Restoring parameters from ./lenet
New Image 3
New Image Accuracy = 0.333

INFO:tensorflow:Restoring parameters from ./lenet
New Image 4
New Image Accuracy = 0.500

INFO:tensorflow:Restoring parameters from ./lenet
New Image 5
New Image Accuracy = 0.600

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Stop Sign      		| Stop sign   									| 
| U-turn     			| U-turn 										|
| Yield					| Yield											|
| 100 km/h	      		| Bumpy Road					 				|
| Slippery Road			| Slippery Road      							|


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of ...

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a stop sign (probability of 0.6), and the image does contain a stop sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .60         			| Stop sign   									| 
| .20     				| U-turn 										|
| .05					| Yield											|
| .04	      			| Bumpy Road					 				|
| .01				    | Slippery Road      							|


For the second image ... 

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


