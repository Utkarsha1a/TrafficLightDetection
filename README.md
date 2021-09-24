# Traffic Light Detection System using Computer Vision Technology
Currently, demand for advanced driver-assistance systems (ADAS) that help with monitoring, warning and braking is expected to increase  and consumer interest in safety applications that protect drivers and reduce accidents. Traffic signal detection, from an image or video, is a vital part of any intelligent system that deals with the maintenance of traffic rules and regulations. Organizations that deal with traffic laws and regulations require an intelligent system which when given an input image is able to determine the presence of a traffic signal, the type of light that is lit up on the signal and warning given to the driver. Currently, a low cost traffic light detection, and vehicle safety system is the need of the hour.

This project uses the YOLO (you only look once) and computer vision technology to detect the traffic light. YOLO is popular because it achieves high accuracy while also being able to run in real-time. The algorithm “only looks once” at the image in the sense that it requires only one forward propagation pass through the neural network to make predictions. 

This project is based on recognition of the traffic light that is lit up in a traffic signal. It can be used by the video camera that captures the nearby surroundings of the driving car and warns the driver to stop the car by hearing the beep sound as the red lights lit up. Results show that this method is not only easy to operate but also has a high accuracy rate for the detection of traffic light type.
  
# YOLO for Object Detection
Object Detection is a task in computer vision that focuses on detecting objects in images/videos.The “You Only Look Once,” or YOLO, family of models are a series of end-to-end deep learning models designed for fast object detection, developed by Joseph Redmon.

# What YOLO can detect
1. Image file
2. Webcam Feed
3. Video File

# Pretrain Yolo can detect 80 objects such as:
Bicycle,Person,Car,Dog,Truck,Horse,etc

# Custom Object Detection Model With Yolo
As the pretrain Yolo model does not have traffic light as an object, we have to prepare our own dataset.

#### How to Train A Custom Object Detection Model with YOLO

**Step1:** Collecting Our Training Images: The dataset consists of approx 100 images.

**Step2:** Annotating Our Training Images:-The images were labeled by using Labelimg. It has 3 classes such as **stop, slow-down and go**.

**Step3:** Prepare and zip image dataset:- After annoting,prepare the images.zip the file.

**Step4:** Setup Google drive- Create a folder **yolov3** and upload the images.zip 
into the folder yolov3

**Step5:** Setup google Colab:- Create a new jupyter notebook to train the yolov3.

**A.** Mount Google drive files in colab
```
from google.colab import drive
drive.mount('/content/gdrive')
!ln -s /content/gdrive/My\ Drive/ /mydrive
!ls /mydrive
```
*Check the google drive map in the colab directory*

**B.** Clone, Configure and compile AlexeyAB Darknet
```
!git clone https://github.com/AlexeyAB/darknet
```
*This will upload the darknet folder in the directory*

**C.** Enable GPU and OpenCV
```
%cd darknet
!sed -i 's/OPENCV=0/OPENCV=1/' Makefile
!sed -i 's/GPU=0/GPU=1/' Makefile
!sed -i 's/CUDNN=0/CUDNN=1/' Makefile
!make
```
*If you check the Makifile in darknet folder, the above code will replace opencv=0 to opencv=1*

**D.** As we have 3 classe **stop,slow-down and go**, we need to make changes in /darknet/cfg/yolov3_training.cfg.
```
# Change lines in yolov3.cfg file
!sed -i 's/batch=1/batch=64/' cfg/yolov3_training.cfg
!sed -i 's/subdivisions=1/subdivisions=16/' cfg/yolov3_training.cfg
!sed -i 's/max_batches = 500200/max_batches = 6000/' cfg/yolov3_training.cfg
!sed -i '610 s@classes=80@classes=3@' cfg/yolov3_training.cfg
!sed -i '696 s@classes=80@classes=3@' cfg/yolov3_training.cfg
!sed -i '783 s@classes=80@classes=3@' cfg/yolov3_training.cfg
!sed -i '603 s@filters=255@filters=24@' cfg/yolov3_training.cfg
!sed -i '689 s@filters=255@filters=24@' cfg/yolov3_training.cfg
!sed -i '776 s@filters=255@filters=24@' cfg/yolov3_training.cfg
```

**E.** Create obj.names file & obj.data file in the data folder
```
!echo -e 'Stop\nSlow down\nGo' > data/obj.names
!echo -e 'classes= 3\ntrain  = data/train.txt\nvalid  = data/test.txt\nnames = data/obj.names\nbackup = /mydrive/yolov3' > data/obj.data
```

**F.** Create obj folder for images inside the data folder and unzip images.zip in google drive yolov3 folder to obj folder 
```
!mkdir data/obj
!unzip /mydrive/yolov3/images.zip -d data/obj
```

**G.** Create train.txt file in data folder
```
import glob
images_list = glob.glob("data/obj/*.jpg")
with open("data/train.txt", "w") as f:
    f.write("\n".join(images_list))
```

**H.** Start training the yolov3 by using below command line
```
!wget https://pjreddie.com/media/files/darknet53.conv.74
```

Running the above steps will train your yolov3 model.
To test the model download the yolov3_testing.cfg, yolov3_training_last.weights, obj.names

#### Testing the Yolov3 Model

**1.** Create classes.txt file having classes name as *stop,slow-down and go* in it.
**2.** Run the TrafficLightDetection.py File 


![Capture2](https://user-images.githubusercontent.com/89068470/134662285-e6909684-c670-48c9-8573-dd174d1ce434.JPG)| ![Capture3](https://user-images.githubusercontent.com/89068470/134662298-d83efbcd-30ac-4e6a-abb3-e434875084c4.JPG)| ![Capture1](https://user-images.githubusercontent.com/89068470/134662300-efe259fb-07fb-4b08-b5ad-5d2cc46329af.JPG)
 
