
#**Finding Lane Lines on the Road** 

[TOC]

---

#Goals
- Make a pipeline that finds lane lines on the road
- Reflect on your work in a written report

---

# Reflection

###1. Description of the pipeline

In the first place, I extract the width and height of the input frame in order to get the vertices that I will use in the ROI function. To calculate this vertices, I wrote a new function, which using a set of locations in percentage values respect to the size, in other words, independent of the size of the image,  returns the vertices for the given frame.
Next I transformed the image to grayscale color map, because lines could be white, yellow or any color depending on the lightening conditions. The grayscale image is then smoothered using a gaussian blur filter, With a kernel size of 5.
Then I apply a canny edge detection to the smooth image. I find out empirically that a low threshold of 150 and a high threshold of 200 have nice results.
The binary image of edges returned, is then cropped using the vertices that i previously calculated, Keeping only a segment of the bottom part of the image.
Subsequently I apply the Hough transform, using the helper function, using a threshold of 5, discarding the segments whose lenghts are beneath 40 px, and have gaps inbetween greater than 80 px.
The helper function then calls to the draw_lines() function which i modified as follows:
First,  I retrieve the width and height of the input image, then construct a blank image with the same dimensions. Next, I get the parameters a and b from the line equation (y = a*x + b), using a custom function get_lines_parameters(), this function segregates the lines according to their slopes. all lines with slopes between -.35 and -2 are considered to be left lines, meanwhile slopes ranging between .35 and 2 will be considered as right lines. the actual output parameteres are the median values of all line parameters. I decided to use the median because is more robust towards noise.
The pipeline continues with an unshaking process, without this method, the lines vibrate between consecutive frames, therefore in order to get more steady representation of the lines, the unshake() function return the mean values for the line parameteres of the last 10 frames.
Finally in persuance of drawing the lines I use the line parameters a and b to construct the left and right lines from the bottom of the picture to 60% of the picture height, and then overlay these lines over the original image.

###2. Output

Output videos could be seen [here](https://www.youtube.com/watch?v=RIERRjmbTtU&list=PL2M3eNbHNxfPizmTBq9fFZnCdAGkHUVPr "Youtube Lane Line Project Playlist")


[//]: # (Image References)

[image1]: /output/solidWhiteCurve_line_overlay.png "Solid white Curve Line"
[image2]: /output/solidWhiteCurve_raw_lines.png "Solid white Curve Line"
[image3]: /output/solidWhiteRight_line_overlay.png "Solid white Curve Line"
[image4]: /output/solidWhiteRight_raw_lines.png "Solid white Curve Line"
[image5]: /output/solidYellowCurve_line_overlay.png "Solid white Curve Line"
[image6]: /output/solidYellowCurve_raw_lines.png "Solid white Curve Line"
[image7]: /output/solidYellowCurve2_line_overlay.png "Solid white Curve Line"
[image8]: /output/solidYellowCurve2_raw_lines.png "Solid white Curve Line"
[image9]: /output/solidYellowLeft_line_overlay.png "Solid white Curve Line"
[image10]: /outputsolidYellowLeft_raw_lines.png "Solid white Curve Line"
[image11]: /output/whiteCarLaneSwitch_line_overlay.png "Solid white Curve Line"
[image12]: /output/solidYellowCurve_line_overlay.png "Solid white Curve Line"
[image13]: /output/whiteCarLaneSwitch_raw_lines.png "Solid white Curve Line"




###3. Potential shortcomings

The first potential shortcoming I could think of, is when the line lanes are not well painted in the street, Which is the case of my local street, therefore I recorded a video and tried to implemented the algorithm with it, it worked well in the majority of the video, nonetheless a single frame with wrong output could potentially drive into an accident.
Another potential shortcomming would be what would happen when there are no line detection for 10 consecutive frames, in which case no line will be displayed.
The program is not desing to work with curves, intersections, speed humps,  potholes, uphills and downhills. 
Overall this algorithm works with ideal climate conditions, it will certainly fail with snow, haze or rain.
reflections on the windshield could also have a negative effect on the output, as well as bright dazzleling lights.
Moreove, if there are objects in the road that could ressemblence to a line, the pipeline would mistake it for a real one. 


###4. Possible improvements

A possible improvement would be to use the vanishing point of the picture to disciminate the correct lines because the vanishing point tends to be very steady in the picture and all lane lines go through it. I can also think of using the gradient of the line parameters to discard all of them that change abruptly between frames.
As an improvement I would restructure the code to use polar coordinates instead of cartesian coordinates, this change will be capable of working with lines with infinite slopes. 
Another possible improvements will be to determine the lines of the adjacent lanes, to be ready at the moment of switching lanes.
Finally I would add an image stabilization algorithm to preprocess the picture.

>*Author*
>Cristóbal Felipe Fica Urzúa
