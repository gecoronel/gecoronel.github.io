---
title: Donde Estan App
description: Mobile Application for school buses
date: "2022-05-20T19:49:05+02:00"
publishDate: "2022-05-20T19:49:05+02:00"
---

As an engineering student, I worked in my family's small school bus company until I graduated. Over time, I visualized different needs that parents of children transported on school buses might have. As a result of these needs, Donde Estan App was born.

{{< figure src="/post/images/donde-estan-app.png" caption="Donde Estan App" >}}

● Necesity

The school transportation service in the city of Santa Fe and surroundings arises from
late 1970s and early 1980s, where it has been growing year after year along with
the increase in school attendance at the initial, primary and secondary levels, and
It has also generated a progressive increase in the demand for the service of
School transportation. Parents of children transported in vans
schoolchildren do not have control of the vehicle or the child once it is
withdrawn from the home and transferred to the school establishment, and vice versa.
In addition, different events can also occur that can cause
delays in it, such as heavy traffic, rain, streets
waterlogged or in poor condition, mechanical failure, tire breakage, among
others. When such events occur, concerned parents begin to
call and send messages to the driver, consulting his location and the cause
caused this delay. At times when these events occur, the driver
is focused on fixing the issue as quickly as possible and may
may not be able to answer all the important queries parents have.
These inconveniences influence a loss of time in the route, which
may result in further delays in finding or returning each child to the
address, or a delay in entering or leaving a school.

Due to this problem, it is intended to make a mobile application that
visualize the geolocation of school vehicles and report, through a
message automatically, in the status bar of your cell phone, the
moment in which the transport arrives at the school or at the private address. East
application work has two types of users, on the one hand, the “users
observers” who are the parents or guardians of the children and, on the other hand,
the “observed users” who are the drivers of school vehicles. Also
The application allows the creation of warning messages to notify, in a way
mass to all observer users, about events that may occur in
the dynamics of a school transport route and there is a channel of
communication between the observed user and the observing users, which
allows the sending and receiving of instant messaging between said users.

● Goals

● General:

○ Develop a mobile application that allows viewing the
geolocation of school transport and send
notifications between observed users and observers.

● Specific:

○ Design and develop the mobile application from the part of
the “observed users”.
○ Design and develop the mobile application from the part of
the "observer users".
○ Develop the web service for processing the
information between Android app and web server
(database).
○ Develop a method for displaying the
geolocation of school vehicles, in order to obtain and
show your location in real time.
○ Develop a method to integrate shipping and viewing
of automatic and massive notifications, in order to communicate.

● Development

1. An analysis of requirements and techniques to be used for the
development of the project, as well as a study on the
different technologies to be used, such as the programming language
JAVA, Android SDK, GPS functionality, Web Service and server
Push Notifications.
2. The tools were selected and the design of the tools was carried out.
class diagrams, database structure, how they will
interact the different parts of the project and the presentation of the
graphical interface through mockups.
3. The development of the database and the web service was carried out, the
which is responsible for exchanging information between
the Android application and the database. In addition, they spread
both developments in AWS services (RDS and Elastic Beanstalk).
4. The functionalities and the graphical interface of the application were developed
mobile.
5. The connection of the mobile application to the services was made
deployed on AWS.

{{< figure src="/post/images/architecture.png" caption="Architecture" >}}

● Results - Conclusions

• As a result of this work, a mobile application was obtained
Android that allows the visualization of the geolocation of
school vehicles and the sending of information in real time between
users through messaging and automatic notifications and
massive.

• It was possible to visualize the geolocation of the driver on a map, as well as
also the sending of automatic arrival notifications and bulk overview notifications. 
In addition, it generated a new communication channel through instant messaging
between users.

• At the same time, and exceeding the proposed objectives, it was possible
make the web service available on AWS.

• In the long term, this project could incorporate new languages,
make the application compatible with iOS devices, incorporate
authentication mechanisms via email and allow the registration of more
of a driver.
