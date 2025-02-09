# Poster Face Transfer Function Documentation

## Content

[TOC]

## 1. Function Introduction

Poster face transfer is a technology that fuses the face in the template image and input image, and returns the fused template image.

## 2. Interface

### Interface Parameters

The interface of poster face transfer is based on props, except**tex_input** and **tex_template**, and other input parameters are implemented through **fuItemSetParam**.

It is divided into general template and preset template. The general template needs to set the following six parameters. The preset template needs to set three parameters related to the input image. The parameters related to the template image are embedded resources, and the preset template cannot load the template image from the outside.

```
		input_width:0,    //Enter the width of the picture (the default is 0, it must be set, otherwise the bundle will return directly)
		input_height:0,   //Enter the height of the image (the default is 0, it must be set, otherwise the bundle will return directly)
		input_face_points:[]   //Enter the feature points of the image, 75 points (the default is empty, must be set, otherwise the bundle will return directly)
		tex_input：//Input the RGBA buffer array of the picture (it is empty by default, it must be set, otherwise the bundle will return directly)
		template_width:0,  //The width of the template image (if it is a preset template, it does not need to be set)
		template_height:0, //The height of the template image (if it is a preset template, it does not need to be set)
		template_face_points:template_face_points,//Feature points of the picture, 75 points (if the preset template does not need to be set)
		tex_template：//RGBA buffer array of template image (if it is a preset template, it does not need to be set)
		warp_intensity:0.5,  //Input face and facial features automatic deformation adjustment, 0-1, 0 is off
```

Now we all use the general template first, and all the parameters need to be passed in by the client.

#### Attention

1. The input of width and height should be carried out before the buffer is passed in
2. The size of the buffer or texture passed in should be consistent with that of the template image when the fuRenderItems is in render.

### tex_template tex_input set buffer method

Now the interface of Nama layer load texture is added. You can use the faster underlying interface fuCreateTexForItem to pass RGBA buffer, or you can use fuDeleteTexForItem to delete the created texture. The created texture will be deleted when the prop is destroyed.

```
/**
\brief create a texture for a rgba buffer and set tex as an item parameter
\param item specifies the item
\param name is the parameter name
\param value rgba buffer
\param width image width
\param height image height
\return zero for failure, non-zero for success
*/
FUNAMA_API int fuCreateTexForItem(int item, char* name, void* value, int width,int height);
/**
\brief delete the texture in item,only can be used to delete texutre create by fuCreateTexForItem
\param item specifies the item
\param name is the parameter name
\param value rgba buffer
\param width image width
\param height image height
\return zero for failure, non-zero for success
*/
FUNAMA_API int fuDeleteTexForItem(int item, char* name);
```

## 3. Make Templates

### Conventional template making

Now to make the template, users only need to find the template image that can recognize the face, and set the above parameters to render the result.

### Steps of making fixed template

You need to fill in the data in the template.json file

The template image file needs to be placed in the same level directory as the template.json file

```
	{
	"template_face_points": [
		460.139, 281.099, 461.997, 334.7, 459.649, 385.568, 448.089, 437.361, 430.167, 486.138, 398.534, 527.839, 360.802, 562.826, 312.593, 576.788, 264.112, 561.675, 225.451, 527.808, 192.342, 485.628, 173.934, 436.938, 164.193, 386.593, 160.835, 335.335, 163.762, 281.267, 193.272, 297.45, 216.613, 277.846, 262.568, 282.069, 284.586, 301.035, 260.418, 301.47, 218.988, 298.087, 439.266, 296.759, 413.345, 278.199, 373.41, 281.171, 350.822, 301.532, 376.127, 300.13, 408.986, 296.957, 418.946, 331.056, 387.135, 317.712, 359.314, 338.212, 389.232, 343.398, 212.428, 330.969, 244.416, 316.241, 272.514, 338.615, 245.014, 345.151, 334.888, 340.615, 340.9, 389.47, 355.585, 426.563, 347.802, 444.577, 315.822, 447.835, 292.118, 444.427, 276.656, 432.457, 288.952, 387.826, 296.36, 343.664, 338.114, 436.746, 296.302, 435.068, 369.892, 480.999, 355.128, 477.993, 334.882, 475.55, 316.562, 477.348, 299.655, 475.351, 280.735, 479.365, 261.091, 483.426, 277.095, 498.78, 298.445, 510.575, 316.022, 511.99, 337.413, 509.556, 355.038, 496.534, 343.342, 491.777, 317.451, 493.385, 294.024, 490.553, 290.918, 487.843, 317.57, 487.565, 345.398, 487.097, 314.939, 427.406, 403.707, 320.587, 403.866, 340.725, 373.28, 342.136, 371.249, 324.301, 259.266, 321.968, 258.617, 344.345, 226.549, 340.722, 226.828, 318.887, 386.426, 329.619, 246.64, 329.785
	],//Length 150, 75 landmarks
	"template_width": 1080,//The width of template image
	"template_height": 1920,//The height of template image
	"image": "mdnl.png"//Template image, with the json file on the same level directory
}
```

#### Attention

Fixed template cannot update template image and data by setting parameters, only input data can be updated

## 4.Attentive Problems 

1. If there are yin and Yang faces in the template image, it will affect the final effect. It is recommended to choose the template image with uniform skin color. If the skin color of the template is uneven, PS can be used for processing.
2. Now for exaggerated expression and large angle template and input image support is not very good, it is recommended to use normal expression face image.





