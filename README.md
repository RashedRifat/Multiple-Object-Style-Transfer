# Multiple-Object-Style-Transfer

All source files in this repository are released under the Apache 2.0 license, the text of which can be found in the LICENSE file.

A project created within the MWML Incubator, the goal of our project is to extend neural style transfer, using multiple styles, to multiple objects identified by the Detectron2 architecture. Previously, neural style transfer was limited to stylizing the entirety of a single image using one style image. In this project, we aim to improve upon the control of this style transfer by allowing users to select which objects to stylize and the style image to be used. Multiple objects can be stylized using different multiple styles (style images can either be uploaded by the user or chosen from a pre-set dictionary of styles) in one pass of the program, resulting in a single output image where objects within the image are stylized according to the specifications of the user. 

View the Google Colab Notebook [here.](https://colab.research.google.com/drive/1-Br4W22PjYB6YYMdXg5vrO_r6ulmfa-V?usp=sharing)


# Demo 

## Fast Style Transfer

Let's see an example of how multiple obejct transfer can be used on an input image. Our project includes this input image by deffault within the program as well as some default style images as well. 

Take the following input image, called the content image. 

![Content Image uploaded to the program](https://github.com/RashedRifat/Multiple-Object-Style-Transfer/blob/master/assets/input_image.jpg?raw=true "Input Image")

In traditional Neural Style Transfer, this content imahge can be stylized by using a style image. The entire content image is transformed and takes on the stylization of the provided style image. An excellent example of this is shown in TF-HubL Fast Style Transfer for Arbitrary Styles [notebook](https://colab.research.google.com/drive/1pZV0a-HVx_XpXtqv7CorWGxndXl1QKi2#scrollTo=jvztxQ6VsK2k) published by the Tensorflow Hub Authors. Our project uses their code to acheive multiple object style transfer. Let's take a look at what Fast Style Transfer can do. (The following in an example for the previously mentioned notebook). 

![Fast Style Transfer Example](https://github.com/RashedRifat/Multiple-Object-Style-Transfer/blob/master/assets/fast%20style%20transfer.png)

While this is quite an excellent result, it does not provide an granular level of control within the content image. Say that we wanted only to stylize the person riding the horse or the horse itself. Perhaps you wanted to stylize the horse with a fire style while the man riding the horse was to be stylized using oceanic waves. This level of control introduces new challenges to the intial project, primarily the usage of computer visison in identifying objects and applying multiple styeles to different objects within the image. Let's take a look at how computer vision is utilized in this project. 

## Computer Vision Using Detectron2

For our project, we use the Detectron2 structure set up by the Facebook AI Research (FAIR) team. The Github Repo can be found [here](https://github.com/facebookresearch/detectron2). 

We implemented the Detetctron2 code in our program to identify the objects in the image and their corresponding object binary masks - a pixel-wise image with pixels being 0 or 1 is the associated pixel in the content image is a part of the mask. 

An example of this object detection can be seen here. 
![Object Detection Example](https://github.com/RashedRifat/Multiple-Object-Style-Transfer/blob/master/assets/object_detection_example.png)

Form here, the appropriate objects, masks and classes can be identified for stylization. 

## Stylizing Multiple Objects Using Multiple Styles 
