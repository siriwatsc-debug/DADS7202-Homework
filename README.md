# DADS7202-Homework
example : https://github.com/lukplamino/DADS7202_HW02-CNN_MNLP_Group

Pizza Classification

DADS7202_Group Assignment 2 CNN (Group_MNLP)
Objective: What do you use to train an image classifier with our custom image dataset?

✨Highlight
Based on pre-trained model, the highest average accuracy rate is 85.37% of 🏆Xception model
The best model after fine tuning pre-trained model is 🏆VGG16. The average accuracy on test data is at 91.66% rising from 84.63% (no tune)
InceptionV3 and VGG16 show notable improvement on test accuracy at +8.71% and +7.04%, respectively, while ResNetV2 and Xception have less improvement at +2.59% and +2.22%, respectively
Overall, the models have learned a distinctive of each banana type from banana fruit shape and shape of banana cut stalk

Table of Contents
1. Introduction🎯
2. Data📑
3. Network architecture📦
4. Training🔮
5. Results📈
6. Discussion💭
7. Conclusion📝
8. References🌐
Citing
👥 Members, Percent Contribution and Responsibility
🖇️End Credit
1. Introduction🎯
multi-class image classification:

This project aims to test 4 CNN pre-training models (VGG16, ResNet50V2, Xception, InceptionV3) on the ImageNet dataset and fine-tune it to classify 3 types of bananas 🍌 (Cultivated banana, Lady finger banana, Cavendish banana) which is our custom image dataset that were never trained on.
Then, we will compare performance of 4 CNN pre-training models without transfer learning and with transfer learning (Fine-tuning).
Finally, we use Grad-CAM technique to debug the model and gain more insight into what a trained CNN did.
2. Data📑
There are many banana0 varieties in Thailand and each one of them has different characteristics. Let’s find out interesting facts about different varieties of banana before training and finetuning models.

🍌 1. Cultivated banana - กล้วยน้ำว้า

The fruit looks a little bit angled with a thick skin and a sweet flavor.



🍌 2. Lady finger banana - กล้วยเล็บมือนาง

Lady finger banana is one of the smallest bananas. Its skin is thick with a soft flesh inside.



🍌 3. Cavendish banana - กล้วยหอม

The fruit is long with a thin skin. It offers a sweet flavor along with a uniquely pleasant smell.



Total 600 images (200 images per 1 class)
Class	Name	No. of image
0	Lady Finger Banana	200
1	CavendishBanana	200
2	Cultivated Banana	200
Total	600
📍Data source:
We use Download All Images extension in chrome web store to collect set of images by searching keywords (3 types of banana) from google image searching
🧹Data preparation:
Collecting set of images from the Internet source is a quick and simple method to gather a set of images. Some facts, meanwhile, are not entirely accurate or useful. As a result, we have to manually remove several unnecessary images from the collection, such as banana dessert, banana tree trunk, other banana pieces, and duplicate images. Additionally, because the keyword and banana type are inconsistent, we need to recheck the banana type labels.
Data pre-processing
Firstly, generate images as in .npy before flow those image into the model
The set of images were rescaled to 224x224 pixels and normalized by the ImageDataGenerator class with Pixel Standardization technique (zero mean and unit variance)
❕ Be careful: The different normalization techniques5 effect the model's performance. (Loss and Accuracy).

We use ➕Data Augmentation technique to increase the diversity of our training set by applying ImageDataGenerator
We apply various changes to the initial data. For example, image rotation, zoom, shifting and fliping
        width_shift_range = 5.0,   
        height_shift_range = 5.0,              
        rotation_range=10,                               
        zoom_range=0.2,
        horizontal_flip=True,                 
        vertical_flip=True,
        validation_split=0.3


✂️Data splitting (train/val/test):
random_state = 3
test_size = 0.3
validation_split = 0.3
Train set: 294
Validation set: 126
Test set: 180
🔝

3. Network architecture📦
Pre-training Models
In this experiment, we have selected 4 Pre-training Models for fine-tuning with IMAGENET6 dataset as weight

Pre-training model Infomation7


Network Architecture of Pre-training model
To compare Network architecture of Pre-training model without Fine-tuning VS with Fine-tuning
Remark: Based on our dataset and our experiment scope


Network Diagram of Pre-training model with Fine-tuning




🔝

4. Training🔮
From the experiment, We fine-tune each pre-training model with Hyperparameter more than 50 times to find the best model's performance with highest accuracy, less loss and not over-fit.
Our training strategy is...


🔝

5. Results📈
📊 Model Performance Comparison
We pre-train the model with initial random weights in the first round and more 2 rounds without random seed to calculate mean±SD of accuracy and loss on test set as the average of the model performance In each round, accuracy and loss of test sets are not significantly different. That proves the model is good fit.



Accuracy and Loss on Train vs Validation sets


Accuracy on Test set
To compare the highest accuracy on test set of each Pre-training models under the same conditions and the same seeds,


The best pre-trained model (No Fine-tune) among the selected models is 🏆Xception with average accuracy rate at 85.37% while after fine tuning it turns out that 🏆VGG16 becomes the best model with the highest accuracy on test set at 91.66% from 84.63%
After fine-tuning, all selected models show improvement on test accuracy average at 🔼5.23%
🪟 Evaluation metric on Test set
VGG16 model with fine-tuning showed that the most precise classification is “Cavendish Banana”.
However, other models (ResNet50V2, Xception and InceptionV3) are good at classifing “Lady Finger Banana”.


⌛ Runtime Comparison (on Train set)
Time per inference step is the average of epoch.

GPU: Tesla T4
Epoch: 30


The overall run-time among the selected model is 5-6s per epochs under GPU usage
InceptionV3 pre-training model with fine-tuning spended the shortest time per epoch to train the model on test set.
On the contrary, Xception pre-training model with fine-tuning spended the longest time per epoch among these 4 models.
🎖️Visualizing bubble chart to compare pre-training models with fine-tuning in all aspects
Based on our dataset and our experiment scope,

The highest average accuracy on test set is 🥇VGG16 model at 91.66%.
The fastest runtime on train set is 🥇InceptionV3 model at 5.278 seconds per epoch on Tesla T4 GPU.
The biggest size is 🥇VGG16 model at 528 Mb.
The most of no. of parameters is 🥇ResNet50V2 model at 75.02mn parameters.


🔦 Visualizing what CNN learned with Grad-Cam4
We use the gradient-weighted class activation mapping (Grad-Cam) technique on VGG16 pre-training model with fine-tuning to understand which parts of the image are most important for classification.
"The Grad-CAM technique utilizes the gradients of the classification score with respect to the final convolutional feature map, to identify the parts of an input image that most impact the classification score. The places where this gradient is large are exactly the places where the final score depends most on the data." 9



For Cavendish bananas (No.4,5) and Cultivated bananas (No.7,8), Clearly, the center of the banana fruit has the greatest impact on the classification.
Contrarily, for all images that the model predicted Lady Finger bananas (No.1,2,6), the model focused on the cut stalk of the banana for distinctive features.
Moreover, we can use the Grad-CAM heat-maps to give us clues into why the model had trouble making the correct classification.
Look at the pictures (no.3,9) with wrong prediction, we can see from the Grad-CAM that the model is emphasizing background or male flowers and having trouble finding banana fruit features.
🔝

6. Discussion💭
With ImageDataGenerator(), apart from shift, zoom, rotation and flip, we have tried rescaling with variety methods. Firstly, rescale = 1./255, it shows the worth accuracy rate at about 0.3611 as it just resize pictures by dividing 255. Next, featurewise_center and featurewise_std_normalization which set dataset mean to 0 and normalized over dataset; as a result, accuracy spike to 0.6278. Lastly, samplewise_center and samplewise_std_normalization is similar to the previous method but done over each input. All in all, we chose samplewise_center and samplewise_std_normalization as it shows the higher accuracy rate at 0.7778
In the beginning of work, we tried load data with flow_from_directory() that labels of picture will be one-hot encoded i.e. [1 0 0 0] for label number 1; consequently, a loss function must be “CategoricalCrossentropy”. However, we decided to load data with NumPy that labeled data from [0-2] i.e [1] for label number 1. As a result, “SparseCategoricalCrossentropy” is chosen to be the loss function
We experience running out of free-access GPU on GoogleColab. So, we try using other accounts, using local GPU on our own computers, and registered for ColabPro since CNN have noticeable difference run-time on CPU and GPU. Execution with CPU roughly takes 40 times of time consuming over GPU (200s vs 5s per epochs)
Among learning rate of 0.01, 0.001, 0.0001 and 0.00001, learning rate at 0.001 shows the best accuracy over test data set Note: result dementated below is tested with InceptionV3 model


Another hyperparameter that affects model accuracy is batch_size, moreover, it varies among models. We use batch_size among 32, 64, and 128
We also struggle with random seed, setting fixed random seed at the top does not affect the following codes i.e. accuracy over test set changes as we repeat run. The solution of this is to set seed to all code that available
🔝

7. Conclusion📝
We select 4 pre-trained models; namely, VGG16, Xception, ResNetV2, and InceptionV3
The best pre-trained model (No Fine-tune) among the selected models is 🏆Xception with average accuracy rate at 85.37% while after fine tuning it turns out that 🏆VGG16 becomes the best model with the highest accuracy on test set at 91.66% from 84.63%
After fine-tuning, all selected models show improvement on test accuracy average at 5.23%
The overall run-time among the selected model is 5-6s per epochs under GPU usage
The most precise classification is “Cavendish Banana”. From Grad-Cam, it shows that the model focuses on banana to determine whether it is Cavendish Banana or Cultivated Banana whereas Lady finger Banana is focused at the cut stalk of the banana for distinctive features by the model
🔝

8. References🌐
Library
Pandas
Numpy
Sklearn
Keras
Matplotlib
Seaborn
Version
Python 3.7.15 (default, Oct 12 2022, 19:14:55) 
[GCC 7.5.0]

NumPy 1.21.6

The scikit-learn version is 1.0.2
The Matplotlib version is 3.2.2
The Seaborn version is 0.11.2
TensorFlow 2.9.2
tf.keras.backend.image_data_format() = channels_last
TensorFlow detected 1 GPU(s):
.... GPU No. 0: Name = /physical_device:GPU:0 , Type = GPU
References
0-. (2019). เรื่องกล้วยๆ. Topspicks.
1-. (2022, Sep 8). tf.keras.preprocessing.image.ImageDataGenerator. TensorFlow.
2Dustin. (2022, Oct 20). [Notebooks update] New GPU (T4s) options & more CPU RAM. Kaggle.
3fchollet. (2020, May 12). Transfer learning & fine-tuning. Keras.
4fchollet. (2021, March 7). Grad-CAM class activation visualization. Keras.
5Jason Brownlee. (2019, Jul 5). How to Normalize, Center, and Standardize Image Pixels in Keras. Machine Learning Mastery.
6Lang, Steven and Bravo-Marquez, Felipe and Beckham, Christopher and Hall, Mark and Frank, Eibe. (2019). IMAGENET 1000 Class List. Github.
7Keras Applications. Keras.
8Training strategy. Neuraldesigner.
9Grad-CAM Reveals the Why Behind Deep Learning Decisions. Mathworks.
🔝

Citing
Bib.file

@Misc{MNLP,
    AUTHOR          = {Navapol San. , Pakawut Kam. , Supisara Poo. , Kantima Tec.},
    TITLE           = {Model : CNN image classification model with finetuning on a custom dataset},
    YEAR            = {2022},
    howpublished    = "\url{https://github.com/lukplamino/DADS7202_HW02-CNN_MNLP_Group.git}"
}
🔝

👥 Members, Percent Contribution and Responsibility
No	ID	Name	% Contribution	Responsibility
1.	6410422002	Navapol San.	25%	Collecting data (Cavendish banana), Fine-tune Model (Xception)
2.	6410422003	Pakawut Kam.	25%	Collecting data (Lady finger banana), Fine-tune Model (InceptionV3)
3.	6410422024	Supisara Poo.	25%	Collecting data (Cultivated banana), Fine-tune Model (ResNet50V2), Summary the Conclution
4.	6410422027	Kantima Tec.	25%	Fine-tune Model (VGG16), Summary the report
🔝

🖇️End Credit
This project is a part of DADS7202 Deep Learning

Term: 1 Year of education: 2022

🎓Master of Science Program in Data Analytics and Data Science (DADS)

🏫National Institute of Development Administration (NIDA)

🔝
