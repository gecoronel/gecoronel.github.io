---
title: Calendar Digitizer
description: Calendar Digitizer using digital image processing
date: "2020-09-10T19:49:05+02:00"
publishDate: "2020-09-10T19:49:05+02:00"
---

Android application Calendar Digitizer applying digital image processing technologies.

● ABSTRACT

With the advancement of technology, many students have decided to start using digital calendars. However, even today important dates such as exam sessions and registration periods are only found in paper format. This document proposes the implementation of different digital image processing methods in order to obtain, from an image of a calendar with marked events, captured by a user, a list with the events by month and the days that belong to that event.

● INTRODUCTION 

Within the scope of the faculty, it is common even today for students to be given at the beginning of each semester different versions of paper calendars that are already marked with different colors to identify each event at a glance, such as: start of shift of exams, registration dates, beginning and end of the semester, among others. In addition, it should be considered that each student can keep their own record of important dates such as partial dates or deliveries of practical work.

However, with the advancement of technology, some students decided to migrate this information to digital calendars that they can carry on their phones and that also send alerts or notifications reminding them of the different scheduled events. The problem that arises is that each student has to manually upload each date from the paper calendar to the digital calendar. A project that could solve this problem would be the development of an application that allows taking a photo of the calendar with the dates marked, identifying each of them and finally migrating them to a digital calendar.

In this work we propose to develop the first part of this application. In other words, it allows loading a photo of the calendar, identifying the marked dates and, as a result, it is expected to present a list by month and by event of the indicated days. To achieve this, it will be necessary to apply different image processing techniques, in the first place to carry out a pre-processing of the photo that is obtained by the user and secondly to detect which days are signaled with events. Within what is the pre-processing of the photo, situations such as the calendar not being centered in the box should be considered. the photo, that it is taken at different distances or with different lighting levels, that the photo is moved and therefore the numbers are not clear or that the photo is rotated at any angle. For the detection of events, on the other hand, each number of each month must be analyzed and determine if within its environment there is any color belonging to the one highlighted by an event. Finally, the application will return a list with dates and events that could then be incorporated into a script that automatically loads them into the digital calendar.


● THEORETICAL FRAMEWORK

In this section we will present the theoretical concepts that were necessary for this project and that will be mentioned later. 

1. Variance of the Laplacian operator for blur detection: the Laplacian of a 2D function is defined as:

{{< figure src="/project/images/variance.png" >}}

Considering an approximation with N4 we obtain a mask of the type:

{{< figure src="/project/images/matrix.png" >}}

2. The Laplacian highlights regions of an image that contain rapid changes in intensity, i.e. edges. To detect the existence of blur we must convolve the image of a channel with the Lapla-cyan kernel stated above, and calculate the variance of the response obtained. If an image contains a large variance, then there is a wide variety of responses, both bordered and borderless, representative of a normal focused image. But if there is very little variance, then there is a small spread of responses, indicating that there are very few edges in the image (the blurrier an image is, the fewer edges there are).

{{< figure src="/project/images/parametrization.png" >}}

which can be rewritten as ρ=xcosθ+ysinθ. Then, it is possible to associate to each line a pair(ρ,θ) that is unique. To detect the angle of rotation, we must iterate thethresh-old of Hough's algorithm until we find an appropriate number of lines that allow us to obtain the most common tilt angle of the image. From Hough's algorithm, associated with the angle θ of the image.

3. Morphological Operations:

• Erosion: The erosion of f by a flat structuring element b at any location (x, y) is defined as the minimum value of the image in the region coincident with b when the origin of is at (x, y). This more detailed definition can be found in the book "Digital Image Processing".

•Dilation: The dilation of f by a flat structuring element b at any location (x,y) is defined as the maximum value of the image in the window bounded by b when the origin of is at (x, y). This more detailed definition can be found in the book "Digital Image Processing".

4. Yen's method: Yen proposes a new analysis of entropies based on correlation entropies, the optimal threshold will be the maximum of the correlated entropy. For further details, see the paper "Survey over image thresholdingtechniques and quantitative performance evaluation"[5] and the article Methods of digital image thresholding based on entropy by Shannon and others[2].

5. Non-local means algorithm: The method is based on in replacing the color of a pixel with an average of the colors of similar pixels. But the pixels most similar to a given pixel have no reason to be close. So it's fair to scan a large part of the image looking for all the pixels that actually resemble the pixel you want to ignore. For more details of the implementation, see "Non-local means denoising"[1].

6. Median filter: To perform a mean filter on a point in an image, we first classify the pixel values ​​in the neighborhood, determine their median, and assign that value to the corresponding pixel in the filtered image. This more detailed definition can be found in the book "Digital Image Processing"[3]. 

7. Binarization by Otsu's method: Otsu's method calculates the threshold value so that the dispersion within each segment is as small as possible, but at the same time the dispersion is as high as possible between different segments. This more detailed definition can be found in the book "Digital Image Processing"[3].

● METHODOLOGY

To solve the proposed problem, some base calendar models were defined. In them, the months must be located 4 per row and in increasing order from left to right with their corresponding name, the weeks may begin on Monday or Sunday indistinctly and the initials of the days must be identifying the columns. The application will be robust enough to accept the calendar in both Spanish and English. A possible calendar model is presented in Figure 1.

{{< figure src="/project/images/calendar-model.png" caption="Figure 1 - Calendar Model" >}}

A. Proposed workflow

Below, in Figure 2, a flow chart is presented that indicates the different steps that were followed and within each one the different methodologies applied are detailed.

{{< figure src="/project/images/flow.png" caption="Figure 2 - Workflow" >}}

B. Description of the stages 

1) Pre-processing: this block will have as input a photograph of the marked calendar uploaded by the user. It must contain a uniform background and without auxiliary elements that can generate noise in the processing of the image, however, it will not necessarily contain the calendar centered, it may be rotated or tilted, contain a background different from the color of the calendar, be far away, or it may be blurred. For these reasons, different image processing techniques will be applied in order to obtain as a result an image that contains only the calendar, upright and with the numbers and letters well defined.

a) We will start by consulting the resolution of the input image to determine if it exceeds the minimum resolution -ima defined, if this happens we will apply a resizing of it and if not the image is discarded.

b) Secondly, we proceed to calculate the degree of blur that the image contains, which can be easily calculated by applying the variance of the Laplacian operator.

c) The next step will be to detect if the image contains adequate lighting. We will do this by calculating the average gray of the image, if the value obtained exceeds a threshold value then the image is considered acceptable, otherwise it is discarded. 

d) Then, we proceed to calculate the rotation angle of the image. For this we calculate the straight lines provided by Hough's algorithm. 

e) We will apply a segmentation to keep the image of the calendar exclusively, discarding everything that does not belong to the calendar. To do this, unbinarization is applied to the image with a threshold and then morphological dilation and erosion operations will be applied. The image obtained is cropped by calculating the first and last values ​​with a value of 255 in the rows and columns. 

f) Finally, some restoration and enhancement operations are carried out to facilitate subsequent processes. The first operation consists of obtaining a threshold using Yen's method. Noise is reduced from this result using the nonlocal means algorithm and impulsive noise is removed using a median filter.

2) Obtaining the months: your objective is to obtain each of the sections of the calendar image corresponding to the 12 months. It is important to note that in the proposed calendar models these have the months and the title spaced apart from each other by a greater amount than their components (name of the month, letter of the day, numbers of the days). The first operation that we will carry out consists in dilating with a 5x5 cross-shaped kernel to try to unify each of these regions that are seeking to separate. Then the white blocks formed are detected, and those that do not meet the following conditions will be discarded:

•The first condition corresponds to the shape of a month: rectangular, wider than it is tall. 

•The second condition corresponds to its area: it is restricted to a maximum determined by the total area of ​​the calendar divided by 12 and to a minimum that was adjusted based on tests at 30% of the maximum.

As a next step, count how many are left. If there were 12 left, it is because the months were found satisfactorily. If there are more left, the same operations are carried out again until all 12 are found. And if there are fewer left, it will not be possible to continue since the calendar photo was taken incorrectly or the calendar format is not adequate, so the user is asked to take the photo again. . Finally, once the positions of the 12 months have been detected, they are cut out and ordered in order to be assigned a number that identifies it (1-January, 2-February, 3-March, etc). To be able to order them, simply take into account the position within the calendar, numbering from left to right and from top to bottom.

3. Component identification: this block will be executed for each of the months. It receives as input the cropped image of the month and its corresponding identification number (1-January, 2-February, 3-March, etc). Its objective is to determine the days of that month and obtain its position within the image, taking into account that the first day of the week can be either Monday or Sunday. Since the title corresponding to the month will not be necessary, the first thing to do is to consecutively apply dilations with a 3x3 kernel in the shape of a cross to obtain a single block in the shape of a cross in order to detect it and crop the image below it. Second, to determine whether the month begins with Monday or Sunday, the region where the letters are found must be obtained. To do this, a row scan will be performed, detecting a change in intensity both at the beginning of the letter and at the end. This region is then passed to pytesseract[4]'s image_to_string() function to detect the first letter and determine the day it starts. To know where the 7 columns are positioned, the region of the image of the letters is dilated until 7 elements are obtained (this will allow the days not necessarily to be represented with a single letter, but also with two or even underlined). The central columns of each of them will be the vertical alignment of the numbers of the month. In a similar way to way to detect the position of the letters, the rows in which the numbers are arranged are detected, only this time, the central position of the rows will be the average between the rows of beginning and end of the numbers. From the first row found, it is determined in which column the first day of the month is found and knowing if the week begins on Monday or Sunday, the calendar year can be estimated. Once the positions of the 7 columns and rows are available, the intersection between them will be the positions where the numbers will be located.

4. Event identification: this block, like the previous one, will be executed for each of the months. It receives the images of the months in color and its objective is to identify the different events and their type (given by the color of the mark). Then, for each of the defined colors, the image of the month is segmented, each of the segmented marks is detected and their positions are obtained. To determine the day(s) covered by said mark, it is simply analyzed whether the environment of the positions it comprises belong to the matrix of the positions of the days returned by the previous module. In the event that it belongs to one, the number of the day of the event will be the one corresponding to the array of the numbers of the days also returned by the previous module. In this way, the different events of the month can be determined according to their type (color) and days.

● RESULTS 

Below are the results obtained after applying each of the stages. First, we present one of the schedules used for testing in Figure 3. Second, Fig. 3. Testing Calendar Instead, the restored calendar of Figure x is passed to the component to obtain the months. 

{{< figure src="/project/images/test-calendar.png" caption="Figure 3 - Test calendar" >}}

Firstly, we obtain the result of applying several dilations, Figure 4, and then we mark the detections obtained in green, Figure 5. Thirdly, we see Fig. 4. 

{{< figure src="/project/images/calendar-with-dilations.png" caption="Figure 4 - Calendar with dilations" >}}

{{< figure src="/project/images/months-detected.png" caption="Figure 5 - Months detected" >}}

Result after applying the dilations, the images corresponding to the months are passed to the component identification module and the position of the days is obtained. Their position is determined as shown in figure 6. 

{{< figure src="/project/images/days-identifications.png" caption="Figure 6 - Identified days" >}}

Finally, the results obtained are presented on the console. corresponding to each of the months. For each Of them, an arrangement corresponding to an event of the month is shown, which contains the following 3 data: number of the month, color with which it is marked, and a list with the days indicated in that color. It can be seen in Figure 7.

{{< figure src="/project/images/console-results.png" caption="Figure 7 - Console results" >}}

● MOBILE APPLICATION

This development has been deployed on a server. A mobile application was developed that allows taking a photo of the calendar and sends it to the web server that processes the image to return a list of the events found, so that events can then be created in the cell phone calendar.

● CONCLUSIONS 

Based on the results presented in the previous section, we can conclude that the desired results were obtained. This is that the user can enter an image of a marked calendar and obtain as output a list with the days, divided by month and by event. However, there are still several conditions that the photograph must have so that the events are detected 100% correctly. These limitations are related to the light with which the photograph was taken, the level of blur in the photograph, the colors used to highlight the different events, the background color of the image and if there is another element in the photo besides the calendar. All these conditions can influence so that the detection of the calendar is not correct, or the detection of the months or the detection of each event and the days associated with it. 
