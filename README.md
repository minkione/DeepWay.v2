## DEEPWAY V2
### Autonomous navigation for blind people.
  
This project is version 2.1 of the DeepWay. See  [v1](https://github.com/satinder147/DeepWay). You can have a look at this [video](https://www.youtube.com/watch?v=qkmU8mN0LwE)
<img src="readMe/cover.png" height=500  hspace=20px vspace=200px/>

**Before proceeding look at demo of version2 at [DeepWay v.2](https://youtu.be/3aPsFYoD9pA)  
Not a 15 minute video person, have a look at this 2 minute trailer [DeepWay v2 -trailer](https://www.youtube.com/watch?v=ks8Rsd65RnM)**
### A question you may have in mind
##### If I already had a repository, why make another ?
* Since V1 was based on keras, and I don't like tensorflow much, so for more control I have shifted to pytorch.  
* It is complete redesign.


### How is it better than others:
1. Cost effective: This version costs approx **400** dollars.
2. Blind people generally develop other senses like hearing very well. Taking away one of their senses by using earphones   would not have been nice so I am providing information to the blind person using haptic feedback.
3. Everything runs on a edge device .i.e on the depth-ai kit.

### Hardware requirements
1. Depth ai kit
2. Arduino nano.
3. 2 servo motors.
4. raspberry pi or any other host device. (Will not be required once depthai kit GPIO support)
5. Power adapter for depth ai kit.
6. 3D printer.(Not necessary)
7. A laptop(Nvidia GPU preferred) or any cloud service provider.

### Installation instruction
1. Clone this repository
2. Install anaconda.
3. Install the required dependencies. Some libraries like pytorch, opencv would require a little extra attention.<br>
> conda env create -f deepWay.yml
4. Change the COM number in the arduino.py file according to your system.
5. Connect the Arduino nano.
8. Compile and run arduino Nano code in the arduino nano.
9. Run blindrunner.py


### 1. Collecting dataSet and Generating image masks.
I made videos of roads and converted those videos to jpg's. This way I collected a dataSet of approximately 10000 images.I collected images from left, right and center view(So automatically labelled). e.g:<br>
<img src="readMe/left.jpg" height=150  hspace=20px vspace=200px/>
<img src="readMe/center.jpg" height=150 hspace=20px vspace=20px/>
<img src="readMe/right.jpg" height=150 hspace=20px vspace=20px/><br>  
    
For Unet, I had to create binary masks for the input data, I used LabelBox for generating binary masks. (This took a looooooooot of time). A sample is as follows-><br> 
<img src="readMe/12.jpg" height=170 hspace=20px vspace=20px/>
<img src="readMe/12_mask.jpg" height=170 hspace=20px vspace=20px/><br>  

**For downloading the labelled data from Labelbox, I have made a small utility named "downloader.py"**
   
### 2. Model training
I trained a lane detection model(Now deprecated) which would predict the lane(left,center,right) I am walking in.  
The loss vs iteration curve is as follows:

<img src="readMe/lane.svg" height=400px/>

I trained a U-Net based model for road segmentation on Azure.
The loss(pink: Train, green: Validation) vs iterations curve is as follows.<br>
<img src="readMe/unet.svg" height=400px/>
<br>
**though the loss is less the model does not perform well**<br>
***I trained a model in keras with a different architecture performs really well
Loss vs iterations curve is:***
<img src="readMe/keras_unet.png" height=400px/>

### 3. 3D modelling and printing
My friend Sangam Kumar Padhi helped me with CAD model. You can look at it [here](https://github.com/satinder147/DeepWay.v2/blob/master/3D%20model/model.STL)

### 4. Electronics on the spectacles
The electronics on the spectacles are very easy. It is just two servo motors connected with a ardunio nano. The arduino nano receives signal from the jetson(using pyserial library), and Arduino Nano controls the servo motors. <br>
<img src="readMe/specs4.jpg" height=400 width=600 align="center" hspace=125px/>
<img src="readMe/specs2.jpg" height=400 width=600 align="center"  hspace=125px/>
<img src="readMe/specs3.jpg" height=400 width=600 align="center"  hspace=125px/>


#### FLOW
1. Get camera feed.
2. Run the image through the road segmentation model.
3. Get lane lines from the segmentation mask.
4. Get depth of all the objects in front of the person.
5. Get all the objects in current lane.
6. Plot all the objects on a 2d Screen.
7. Push the person to the left lane (As per indian traffic rules)
8. Use A-Star algorithm to get a path from current location to 2 meters ahead while maintaining distance from objects.
9. Inform the user about all the navigation instructions by using the SERVO motors.

# People to Thank
1. **Army Institute of Technology (My college).**
2. **Prof. Avinash Patil,Sangam Kumar Padhi, Sahil and Priyanshu for 3D modelling and printing.**
3. **Shivam sharma and Arpit for data labelling.**
4. **Luxonis for providing a free depth ai kit**
5. **LabelBox: For providing me with the free license of their **Amazing Prodcut**.**
6. **Luxonis slack channel**

# References
1. [Pytorch](https://pytorch.org/)
2. [PyimageSearch](https://www.pyimagesearch.com/)
3. [Pytorch Community, special mention @ptrblck](https://discuss.pytorch.org/)
4. [AWS](https://aws.amazon.com/)
5. [U-Net](https://arxiv.org/pdf/1505.04597.pdf)
6. [U-Net implementation(usuyama)](https://github.com/usuyama/pytorch-unet)
7. [U-Net implementation(Heet Sankesara)](https://towardsdatascience.com/u-net-b229b32b4a71)
8. [Hao pytorch-ssd](https://github.com/qfgaohao/pytorch-ssd)
9. [Jetson-hacks](https://www.jetsonhacks.com/)
10. [Tensorflow](https://www.tensorflow.org/)
11. [Keras](https://keras.io/)
12. [Advanced lane detection-Eddie Forson](https://towardsdatascience.com/teaching-cars-to-see-advanced-lane-detection-using-computer-vision-87a01de0424f)

# Citations

> Labelbox, "Labelbox," Online, 2019. [Online]. Available: https://labelbox.com

# Liked it
Tell me if you liked it by giving a star. Also check out my other repositories, I always make cool stuff. I even have a youtube channel "reactor science" where I post all my work.
# Read about v1 at:
1. [Geospatial Magazine](https://www.geospatialworld.net/blogs/now-visually-impaired-can-navigate-easily-with-deepway/)
2. [Hackster](https://blog.hackster.io/navigation-glasses-gently-poke-you-in-the-right-direction-21acc16c8b14)
3. [Anyline](https://anyline.com/news/3-inspiring-ai-projects-we-love/)
4. [Arduino blog](https://blog.arduino.cc/2018/08/20/deepway-helps-the-visually-impaired-navigate-with-a-tap/)
