+++
date = "2020-03-17 07:01:22+00:00"
draft = false
tags = ["python", "yolov3", "covid-19", "pytorch"]
title = "“Don’t touch your face” - Neural Network will warn you"
url = "/post/612817685627191296/dont-touch-your-face-neural-network-will-warn"
+++
A few days back, Keras creator, <a href="https://twitter.com/fchollet/status/1234883862385156098?s=21" target="_blank">Francois Chollet tweeted</a>

> A Keras/TF.js challenge: make a model that processes a webcam feed and detects when someone touches their face (triggering a loud beep).“ The very next day, I tried the Keras yolov3 model available in the Github. It was trained on the `` coco80 `` dataset and could detect person but not the face touch.

<figure class="tmblr-full" data-orig-height="941" data-orig-src="https://github.com/kracekumar/facetouch/blob/master/demo_images/output/malcolm-x.jpeg?raw=true" data-orig-width="1200"><img data-orig-height="941" data-orig-src="https://github.com/kracekumar/facetouch/blob/master/demo_images/output/malcolm-x.jpeg?raw=true" data-orig-width="1200" src="https://66.media.tumblr.com/e3d87b5c06a42b08bc0b4f4d156e18d6/2db36526cdb5e07c-2c/s540x810/a8c55db5ee8071e3165cfa37377d90616d9a3031.jpg" width="500px"/></figure>

### V1 Training

The original Keras implementation lacked documentation to train fromscratch or transfer learning. While looking for alternative implementation, I came across the PyTorch <a href="https://github.com/ultralytics/yolov3/wiki/Example:-Transfer-Learning" target="_blank">implemetation with complete documentation</a>.

I wrote a <a href="https://github.com/kracekumar/facetouch/blob/master/face_touch_download.py" target="_blank">simple python program</a> to download images based on keyword.

> python face\_touch\_download.py –keyword "chin leaning” –limit 100 –dest chin\_leaning

The script downloads 100 images and store the images in the directory. Next, I annotated the images using <a href="https://github.com/tzutalin/labelImg" target="_blank">LabelImg</a> in Yolo format. With close to 80 images, I trained the network on a single class `` facetouch `` for 100 epochs. The best of the weights couldn’t identify any face touch on four images. Now, I wasn’t clear should I train all classes(80 +1) or need more images with annotation.

### V2 Training

Now, I downloaded two more sets of images using two different queries `` face touch `` and `` black face touch. ``After annotation, the dataset was close to 250 images. Then I decided to train along with a few other original classes of coco labels. I selected the first nine classes and face touch. Now, the annotated `` facetouch `` images need a change in class. For example, in V1, there was only one class(0), now those annotations are misleading because some other class takes up the number, now 0 means person and9 means facetouch. Also, I picked random 500 images from the original coco dataset for training. After remapping the existing annotation and creating a new annotation, I trained the network for 200 epochs.

After training the network, I again ran the inference on four sample images - Dhanush rolling up mustache (1),another person rolling up mustache (2), Proust sitting in a chair leaning his face on the chin(3), andVirginia wolf leaning her face on the chin(4). The network labeled the person rolling up mustache, second image as face touch.First Victory! At the same time, the network was not predicting other images for classes person and face touch.

<figure class="tmblr-full" data-orig-height="715" data-orig-src="https://github.com/kracekumar/facetouch/blob/master/four_images.png?raw=true" data-orig-width="794"><img data-orig-height="715" data-orig-src="https://github.com/kracekumar/facetouch/blob/master/four_images.png?raw=true" data-orig-width="794" src="https://66.media.tumblr.com/af5744c2f5e8c678e624b2bd5935be35/2db36526cdb5e07c-92/s540x810/cc4288fc115b1b4f8435892d7db17609f6413f6f.png" width="500px"/></figure>

### V3 Training

After re-reading the same documentation again, it was clear to me, the network needs more training images, and _needn’t be trained_along with the rest of the classes. Now I deleted all the dataset v2 and annotations and came up with a list of keywords - `` face touch, ```` black face touch, `` `` chin leaning. `` After annotation of close to 350 images, running the training,this time network was able to label two images - Another person rolling up mustache (2),Proust is sitting in a chair, leaning his face on the chin(3).

By now, it was clear to me that yolov3 can detect chin leaning andmustache as long as the rectangular box is of considerable size and next step.

### Final Training

Now the list of keywords grew to ten. The new ones apart from previous ones are `` finger touching face, ```` ear touch, `` `` finger touching forehead, `` `` headache, `` `` human thinking face, `` `` nose touching, ```` rubbing eyes. `` The <a href="https://drive.google.com/drive/u/0/folders/17-rLAQ9GLda7M5mvDiitINqSLBUj_wbm" target="_blank">final dataset</a>was close to 1000 images. After training the network for 400 epochs on the dataset, the network was able to identify face touch on videos, images, and webcam feed.

Classification (bounding box) on 17 images

<figure class="tmblr-full" data-orig-height="715" data-orig-src="https://github.com/kracekumar/facetouch/raw/master/test_images.png" data-orig-width="790"><img data-orig-height="715" data-orig-src="https://github.com/kracekumar/facetouch/raw/master/test_images.png" data-orig-width="790" src="https://66.media.tumblr.com/b09d2d542e8c85ec510ba269bf79f8d8/2db36526cdb5e07c-0e/s540x810/b4db4f13648a2cc2d81f2f1e1932ce6b7d890402.png" width="500px"/></figure>

Out of 14 face touching images, the network able to box 11 images. The network couldn’tbox on two Murakami images.

Here is a youtube link to identifying facetouch on Salvoj Zizek clip - <a href="https://www.youtube.com/embed/n44WsmRiAvY." target="_blank">https://www.youtube.com/embed/n44WsmRiAvY.</a>

<iframe allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="315" src="https://www.youtube.com/embed/n44WsmRiAvY" width="560"></iframe>

### Webcam

Now that, the network predicts anyone touching the face or trying to touch the face, you can run the program in the background, it warns you with the sound, `` Don't touch the face. ``

After installing the <a href="https://github.com/kracekumar/facetouch#installation-mac" target="_blank">requirements</a>, run the program,

> python detect.py –cfg cfg/yolov3-1cls.cfg –weights best\_dataset\_v5.pt –source 0

You’re all set to hear the sound when you try to touch the face.

The <a href="https://github.com/kracekumar/facetouch" target="_blank">repo</a> contains a link to the trained model, dataset, and other instructions. Let me know your comments and issues.

I’m yet to understand how yolo works, so far, only read yolo v1 paper.The precision for the model is around ~0.6. I think it’s possible to improve the precision of the model. Somewhere while transferring the image from the server, the `` results.png `` went missing.

### Few Issues

*

    If you run the `` detect.py `` with webcam source, the rendering sound is blocking code, you may notice a lag in the webcam feed.


*

    Sometimes, the model predicts the mobile phone in front of the image as a face touch.


*

    As shown in the images, if the face touch region is tiny like a single index finger over the chin, it doesn’t predict, look at the below image titled 6.



<figure class="tmblr-full" data-orig-height="715" data-orig-src="https://github.com/kracekumar/facetouch/raw/master/test_images.png" data-orig-width="790"><img data-orig-height="715" data-orig-src="https://github.com/kracekumar/facetouch/raw/master/test_images.png" data-orig-width="790" src="https://66.media.tumblr.com/b09d2d542e8c85ec510ba269bf79f8d8/2db36526cdb5e07c-0e/s540x810/b4db4f13648a2cc2d81f2f1e1932ce6b7d890402.png" width="500px"/></figure>

*

    The quality of the images was slightly issued because of the watermarks and focus angle. If the end deployment targets at the webcam, then it’s better to train image with front face focus. There was no clean way to ensure or scrap the high-quality images. Do you know how to solve the problem or even this assumption is true?. Of course, good model should detect face touch from all sides and angles.


*

    I haven’t tried augmenting the dataset and training it.



The network may not run on edge devices, you may want to train using yolov3-tiny weights.

*   Yolo site: <a href="https://pjreddie.com/darknet/yolo/" target="_blank">https://pjreddie.com/darknet/yolo/</a>
*   PyTorch yolov3 implememtation: <a href="https://github.com/ultralytics/yolov3" target="_blank">https://github.com/ultralytics/yolov3</a>
*   Facetouch Repo: <a href="https://github.com/kracekumar/facetouch" target="_blank">https://github.com/kracekumar/facetouch</a>

While writing the code and debugging, I must have touched my face more than 100 times. It’s part of evolution and habit, can’t die in few days. Does wearing a mask with spikes help then?
