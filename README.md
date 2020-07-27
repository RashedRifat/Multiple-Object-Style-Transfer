# Multiple-Object-Style-Transfer

All source files in this repository are released under the Apache 2.0 license, the text of which can be found in the LICENSE file.

A project created within the MWML Incubator, the goal of our project is to extend neural style transfer, using multiple styles, to multiple objects identified by the Detectron2 architecture. Previously, neural style transfer was limited to stylizing the entirety of a single image using one style image. In this project, we aim to improve upon the control of this style transfer by allowing users to select which objects to stylize and the style image to be used. Multiple objects can be stylized using different multiple styles (style images can either be uploaded by the user or chosen from a pre-set dictionary of styles) in one pass of the program, resulting in a single output image where objects within the image are stylized according to the specifications of the user. 
