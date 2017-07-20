# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of the following steps. First I am loading the image, then I am converting it to grayscale, then I'm applying a blur filter (with filter kernel 5); then I am detecting the edges in the picture (canny) and select the region of interest. In the next step I am applying the hough transform to search for relevant line segments and to compute from these the left and right lane representations. These are then safed to a separate image (in red) together with the candidate segments (in green). This "hough image" is then overlayed with the original image.

I shifted all the predefined functions (gaussian_blur, canny, etc.) into a single class (imgproc). I did this because I wanted to filter the result from the draw_lines function (which I included in the hough transform function code). With this setup I can instantiate the imgproc class once (globally) before starting a video and can then load new images into the class (frame = imgproc.load(...)) while keeping e.g. the filter variables separately accessible.

I included the draw_lines code into the hough transform function. I modified the draw_lines() function by selecting candidates for left and right lane (distinction by gradient) and weighted the candidate segments acoording to their length. From these weighted segments I compute the left and right lane for the current frame. If a suitable lane candidate exists in the frame, this input is used as measurement input for a kalman filter which is used to filter when a mesurement exists and to predict when no measurement is available.

### 2. Identify potential shortcomings with your current pipeline

I spent much too little time on error checking. Sorry!!!

On the lane with the interrupted lane markers not many candidate segments are found => I have to tune the image processing functions to achieve better results

Another potential shortcoming is that the model for the line representation is linear - therefore I think there will be problems with strong curves, crossing, etc.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to include knowledge about vehicle motion in order to make the lane prediction step more robust

The selection vertex that is used to select the relevant part of the image frame could be shifted dynamically (e.g. depending on the steering wheel angle)

