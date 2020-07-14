 the# Face Mask Detection using Mask R-CNN with Supervise.ly for data Management
In this repo, I have developed a Face mask detection model using mask R-CNN. In machine model, we needed dataset, and it is tough to manage all data, specially when it is image data set. There are plenty of pre-processing operation that we need to do. Some of them are:- create classes of object we are searching for, Augmentation of Image dataset( if we have less dataset).
For Data Management, we will use supervise.ly. It is a simple tool to work with. And do most of the pre-processing work in no time and less effort.

All work can be summarized in 3 steps:-

 1. Create or download image dataset
 2. Use Supervise.ly to pre-process and model creation
 3. Train the Model on our dataset
### Dataset Collection
First thing for any machine learning model is data collection. Best website to look for data is Kaggle. But I had some images for this model. You can use Kaggle dataset. 
Link to Kaggle dataset - [Kaggle Face mask Dataset](https://www.kaggle.com/shreyashwaghe/face-mask-dataset)
Now download this data in your pc.
### Data Pre-processing Using Supervise.ly
To Use supervisely, One should have supervisely account. If you don't have supervisely account, go and create one. It's free of cost.
After log in to your account
Click on **CurrentTeam**  which will lend you on below image
![Workspace](https://github.com/narayanhari/MLOPSTask6/blob/master/1.png)
Click at add button and create a new workspace with name Mask Detection
Now click on Mask Detection workspace, you will see three options.
![Upload Dataset](https://github.com/narayanhari/MLOPSTask6/blob/master/2.png)
Now select Import data to upload the data we downloaded from Kaggle. It's a simple drag and drop option. Give a suitable project name, i.e. Mask Detection.
Now, after uploading, go to projects and select your project. Where you will able to see dataset you just uploaded.  
![before augmantation](https://github.com/narayanhari/MLOPSTask6/blob/master/before%20augmantation.png)
I have 23 images, and the count is shown there as well.
Now We have to select mask from each image to create class of mask.
Click on the images folder in dataset section of your project.
![Mask selection](https://github.com/narayanhari/MLOPSTask6/blob/master/3.png)
Now here you will manually detect the mask in each of your images to create mask class. For this select polygon shape from left window and create boundaries around each mask in the image. Do this for all images.
After doing this for all images, go back to your project.
Now its time to write augmentation script. Click on `3 dots->Run DTL->Create Train set->Instance segmentation`
Now an editor will open to edit JSON script which can be used to augment the images. as sample script is uploaded on [this repo](https://github.com/narayanhari/MLOPSTask6/blob/master/augmantation_DTL.json)
After this click on **start**. 
![after augmantaion](https://github.com/narayanhari/MLOPSTask6/blob/master/after%20augmantation.png)
You can see initially I have 23 images, and after augmentation Now I have 484 images. It is insanely effective when you have less dataset.
Now data set is prepared.
Now time to select which model to use for training our model. We will use transfer learning with Mask R-CNN.
Select Neural Network from left pane. Select Add then chose Mask R-CNN(TF+Keras). 
Now All thing is set. Just final step is remaining, that is training of our model. 
### Model Training
Supervisely does not provide resources to train our model. It is like Bring your own Resources. Here I will show how to use google cloud platform to train the model.
First select cluster from left pane. Then click on Add this will lend you on below screen.
![adding agent](https://github.com/narayanhari/MLOPSTask6/blob/master/adding%20agent1.png)
Now we need agent to train our model.
Agent can be any machine with GPU enabled. Hence we will use GCP platform for creating virtual machine.
**Note:- GCP does not provide free GPU service, you have to pay for GPU**
First login to your GCP console.
If you are new to GCP, first you have to increase limit to use GPU.
To increse Limit navigate to `IAM & Admins -> Quotas` 
Add filter like Service-Computer Engine API & Location where GPU is provided.
Now Select a GPU you want to use. I choose NVIDIA k80. Click on edit quotas and request for limit increment.
After a while, you will get a mail.
Once the limit is increased. Its time to create a virtual machine.
Navigate to `Coputer Engine->VM Instances->create instance`
Fill Name, region and machine configuration.
To add GPU to your VM, click on `CPU platform and GPU.`
Add GPU( whose limit you increased).
![Cre virtual machine](https://github.com/narayanhari/MLOPSTask6/blob/master/6.png)
Once the instance is created, connect to the instance and Copy paste the command that is shown by supervisely at add agent screen.
Rest of the work will be done by supervisely. It will download docker image and train you model. 
The last step is to test the model. Click on the test button (next to train) to test our model.
![enter image description here](https://github.com/narayanhari/MLOPSTask6/blob/master/7.png)

That's it Mask Detection Model is ready.
