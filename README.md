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

![Object Detection Example](https://github.com/RashedRifat/Multiple-Object-Style-Transfer/blob/master/assets/object_identification_example.png)

From here, the appropriate objects, masks and classes can be identified for stylization. 

## Creating Masks 

After each object and its corresponding mask has been identified appropriately, multiple styles are chosen by the user. The user will then identify which objects should be attributed to each particular style. Again, multiple objects can be attributed to a single style. 

Here, the objects of each style have their masks combined to form a single aggregrate mask for each style. The end result is a list of styles and aggregreate masks for each particular style. An example of an aggregrate mask has been shown below, which contains multiple objects within it. True values are shown in yellow while false values are shown in purple. 

The final step is to create an inverse mask, which can be accomplished by creating a tensor of ones of the same shape as the content image and subtracts the masks of all selected objects. The end result is an inverse mask that can extract all pixels that should not be stylized. 

## Extracting Objects from Stylized Images 

Here, the content image and style images are passed into the neural style transfer architecture. This will result in multiple content images, each stylizied according to the input specfications. 

In order to extract the the appropriate objects from each style, the aggregrate masks for each style can be broadcast into the stylized content images. Doing so results in an image where only the objects for each associated style are extracted. The net result is a list of images, each containing objects of a single style. The example below shows extraction of several objects from a stylized content image. 

Finally, an inverse image is created. This consists of the content image and the inverse mask being broadcasted together. This results in an image where all chosen objects are absent. In essence, this is the background for our stylized objects. 

An inverse image is shown below. Note that more objects are absent than the stylized objects shown above. This is because the missing objects are being stylized using a different one than the style above. 

## Creating the Final Image 

In order to build a final image, each of the stylized object images must be combined along with the inverse image (background). This can be done simply by adding each object image onto the inverse image (as the shapes of each image has been preserved). In practoce, each object image can be thought of as a layer and this is the process of merging each layer to have the net effect of reconstructing the final image. This results in a full image, where all pixels have been filled in, creating the true stylized image. 

This is the resulting image when three differnt styles have been applied to all the objects within one image.

An important note to make here is that layers must be pixel-exclusive; otherwise fringe effects are prone to occur. 


# Limitations and Issues 

The limitation of this program can be easily identified: it stems from the capabilites of the two models it extends. The accuracy to which each object is identified is limited by the efficacy of the Detectron2 model. Similarily, the degree to which each image can be stylized depends on the fidelity of the neural style transfer architecture. 

It is recommended that images of size 640 x 640 be chosen. While this program is fully capable of usiing higher resolution images, it becomes more and more computatinally expensive. We were unable to test this limitation due to our limited hardware capabilites. 

Another limitation of this program is that it does not provide a trainable model. In pursuit of greater speed and efficiency, this capability was compromised. Both the Detectron2 and nerual stlyle transfer architecture have been pre-trained on the COCO dataset. 

Finally, some deriative issues arise when these models are applied in conjunction. Some portions of the resultant image might become oddly distorted. This results when two layers occupy the same space - the result of adding these two layers creates an interfrence that distorts the final image. This originates from the Detectron2 structure, where objects may be identified with pixels being attributed to more than a single object. This effect carries over the course of the program and results in layers which are not pixel exclusive. One solution to this could be to re-train the Detecron2 on a particualr image. This will compromise in speed while increasing accuracy. The authors are attempting to address this issue in further edits. 




