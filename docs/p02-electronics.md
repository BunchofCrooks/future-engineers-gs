---
title: Electronics 
summary: Electromechanical components, controllers and optics
authors:
    - Alexander Kolotov
---
# Electromechanical components

Here are materials that aimed to help understand how to make the vehicle is smarter than an RC toy.

## Design leading questions

Definitely, the obstacles detection and decision making for the motion planning must be done ad-hoc by en electronic brain rather than by a human being. That's why it is needed to choose the set of controllers, electronic components and design the logic to connect them to each other. They will mostly rely on the way how the vehicle will solve the task. But some ideas will be common due to the nature of the challenge.

Here is the set of questions that is intended to boost the process of thinking about electronic filling of the autonomous car:

  1. Will a camera be used to recognize the traffic signs (red and green pillars)? Is it an USB-camera or a natively plugged device (like the Raspberry Pi camera)?

  2. How fast the video stream from the camera must be handled? What is suitable performance of the decision making controller?
  
  3. Will the vehicle be controlled by one controller or by a pair of the main controller and an auxiliary controller? How will the roles between controllers be distributed: handling of the video stream, direct control of the motors, direct acquiring of sensors readings? Will capacity (input/output pins) of each controller be enough for this? What kind of connection will be between the controllers? 
  
  4. Will the walls be detected by camera or by a distance/proximity sensors?
  
  5. Should a special lens kit be used to make the camera's field of view wider to recognize both traffic signs and walls or several cameras will be used instead?
  
  6. How will the passed path be measured: by counting numbers of turns with a compass or a gyroscope, by calculating the distance with an encoder(s), by detecting colored lines with light sensors? 
  
  7. Will a gyroscope and/or an accelerometer be used to assist in accurate driving -- get rid of excessive steering mechanism shifts?
  
  8. What kind of power sources should be used to supply controllers, motors and senors? Should it be one source for all consumers or dedicated sources for controllers and motors?

## Overview

Before start drawing schemes and purchasing the components, usually it is worth to review existing solutions: what electromechanical components are the common practice to use for the robotics projects where the aim is to create a self-driving vehicle.

![Arduino based autonomous car](https://cdn.instructables.com/ORIG/FJZ/WMJX/JH0TM5AR/FJZWMJXJH0TM5AR.jpg?auto=webp&frame=1&width=640&height=480&fit=bounds&md=3c0d16be475260e77654ba49daa5f252){target=_blank}

This is a short collection of the links to study. Descriptions of these projects provides different level of details that's why we list several of them to demonstrate diversity of approaches:

  * [Is your toy RC car is good to become autonomous robotics car?](https://diyrobocars.com/2020/06/14/the-difference-between-proper-rc-cars-and-toys-when-youre-turning-them-into-robots/){target=_blank}

  * [A good start to turn an RC car to Arduino controlled car](https://create.arduino.cc/projecthub/GeekRex/turn-your-rc-car-to-bluetooth-rc-car-1b0689){target=_blank}

  * [An example of equipping an RC car with sensors and Arduino controller](https://makezine.com/projects/build-android-powered-autonomous-rc-car/){target=_blank}

  * Not sure how to start driving autonomously? Start with these fours simple turtle-like car based on Arduino: [1](https://www.instructables.com/id/How-to-Build-Arduino-Self-Driving-Car){target=_blank}, [2](https://trybotics.com/project/Self-Driving-Car-Using-Arduinoautonomous-Guided-Ve-29052){target=_blank}, [3](https://project.seeedstudio.com/31926/arduino101-ble-autonomous-rover-2cb19f){target=_blank}, [4](https://howtomechatronics.com/tutorials/arduino/arduino-robot-car-wireless-control-using-hc-05-bluetooth-nrf24l01-and-hc-12-transceiver-modules/){target=_blank}

  * [Yet another DIY autonomous vehicles but pay attention on the powering](https://www.instructables.com/id/Autonomous-RC-Car/){target=_blank}

## Vehicle's brain

If this is the first time when you meet with [Single Board Microcontrollers](https://en.wikipedia.org/wiki/Single-board_microcontroller){target=_blank} like Arduino or [Single Board Computers](https://en.wikipedia.org/wiki/Single-board_computer){target=_blank} like Raspberry Pi, consider to spend extra time to understand the difference between them. For example, the goal of [this article](https://roboticsbackend.com/when-to-use-arduino-vs-raspberry-pi/){target=_blank} to summarize advantages of SBM and SBC with advices when use Arduino, when Raspberry Pi and when both.

Okay, hope the advantages are clear, and that is why it make sense to compare the Raspberry Pi with other SBCs:

[![Raspberry Pi or another SBC?](https://img.youtube.com/vi/G-w7ycyd8tA/0.jpg)](https://youtu.be/G-w7ycyd8tA){target=_blank}

_Click on the picture to watch the video_

Another interesting approach could be is to use and integrated Computer Vision boards instead of SBC. [This material](https://diyrobocars.com/2018/06/05/comparing-three-low-cost-integrated-computer-vision-boards-for-autonomous-cars/){target=_blank} compares OpenMV, Pixy and Jevois.

Someone already decides to build the vehicle with two controllers: the primary controller is to execute navigation, path planning and work with a camera, the secondary is to requests sensors and/or control motors. Most probably the secondary controller will be an Arduino based and the following step is learn how it could be connected to the primary one. There are several protocols that can be used for this and [this document](https://dronebotworkshop.com/i2c-part-2-build-i2c-sensor/){target=_blank} is good to start getting familiar with them.

Next, depending on the protocol the following materials will help you implement communication channel between Raspberry Pi an Arduino:

  * Serial communication: [1](https://roboticsbackend.com/raspberry-pi-arduino-serial-communication/){target=_blank}, [2](https://www.instructables.com/id/Connect-Your-Raspberry-Pi-and-Arduino-Uno/){target=_blank}, [3](https://www.dummies.com/computers/raspberry-pi/connecting-the-raspberry-pi-and-the-arduino/){target=_blank}, [4](https://roboticsbackend.com/raspberry-pi-arduino-serial-communication/){target=_blank}.

  * I2C communication: [1](https://gonzalo123.com/2017/05/22/arduino-and-raspberry-pi-working-together-part-2-now-with-i2c/){target=_blank}, [2](https://dronebotworkshop.com/i2c-arduino-raspberry-pi/){target=_blank}, [3](https://howtomechatronics.com/tutorials/arduino/how-i2c-communication-works-and-how-to-use-it-with-arduino/){target=_blank}.

## Motors

The minimal number of motors that required for a vehicle participating in the Future Engineers competition is two: a driving motor and a motor to steer. 

  * [Motor Basics](https://www.deviceplus.com/arduino/entry011/){target=_blank}
  * [Motor Driver](https://www.deviceplus.com/arduino/entry012/){target=_blank}
  * [Using a Servo Motor for the Steering](https://www.deviceplus.com/arduino/entry013/){target=_blank}

The driving motor makes the car moving. This motors consumes more power than can be provided either by Arduino or by Raspberry Pi therefore an additional motor driver circuit is being used. Here are examples how to control DC motors from Arudino: [1](https://howtomechatronics.com/tutorials/arduino/arduino-dc-motor-control-tutorial-l298n-pwm-h-bridge/){target=_blank}, [2](https://dronebotworkshop.com/dc-motors-l298n-h-bridge/){target=_blank}, [3](https://www.bc-robotics.com/tutorials/controlling-dc-motor-arduino/){target=_blank}; or from Raspberry Pi: [1](https://maker.pro/raspberry-pi/projects/controlling-a-dc-motor-with-raspberry-pi4-1){target=_blank}.

In order to understand better how to control speed of a DC motor it makes sense to look at [this article](https://howtomechatronics.com/how-it-works/electronics/how-to-make-pwm-dc-motor-speed-controller-using-555-timer-ic/){target=_blank}.

As an alternative to the DC motors, one could consider to use [a stepper motor](https://learn.adafruit.com/adafruit-arduino-lesson-16-stepper-motors/overview){target=_blank} or [a continuous servomotor](https://cdn.sparkfun.com/assets/resources/4/4/servos_with_Arduino_slides.pdf){target=_blank}.

A servo motor is the most suitable way to change direction of the vehicle since it provides precise control on the steering process. Here are examples how to control DC motors from Arudino: [1](https://howtomechatronics.com/how-it-works/how-servo-motors-work-how-to-control-servos-using-arduino/){target=_blank}, [2](https://www.circuitbasics.com/controlling-servo-motors-with-arduino/){target=_blank}, [3](https://dronebotworkshop.com/servo-motors-with-arduino/){target=_blank}, [4](https://dronebotworkshop.com/analog-feedback-servo-motor/){target=_blank}; or from Raspberry Pi: [1](https://maker.pro/raspberry-pi/tutorial/how-to-control-servo-motors-by-tilting-your-smartphone){target=_blank}.

### Encoders

It is important to control the driving motor speed and the distance the vehicle is moving. One of the simplest device for this is an encoder:

[![Tracking distance with encoder](https://img.youtube.com/vi/cLtMcqRetO0/0.jpg)](https://www.youtube.com/watch?v=cLtMcqRetO0){target=_blank}

_Click on the picture to watch the video_

These three articles will help you better understand how encoders work: [1](https://www.seeedstudio.com/blog/2020/01/19/rotary-encoders-how-it-works-how-to-use-with-arduino/){target=_blank}, [2](https://dronebotworkshop.com/rotary-encoders-arduino/){target=_blank}, [3](https://www.instructables.com/id/How-to-Use-an-Rotary-Encoder-With-Arduino/){target=_blank}. 

And here are practical examples how an encoder can be added to the autonomous vehicle:

  * [Setup an encoder on a chassis](https://diyrobocars.com/2020/01/31/how-to-add-an-encoder-to-the-donkeycar-chassis/){target=_blank}
  * [Use encoder built in the motor](https://www.allaboutcircuits.com/projects/use-an-arduino-to-control-a-motor/){target=_blank}

## Distance sensors

Distance and proximity sensors are commonly used in robotics. For the autonomous vehicle participating in the Future Engineers competition such kind of devices will allow to control the distance to the walls as so the car will move in the middle between them.

Here is a set of short reviews of different sensors (ultrasonic, infrared and LiDAR) describing the principles how they work and which advantages and disadvantages they have: [1](https://diyprojects.io/hc-sr04-ultrasound-vs-sharp-gp2y0a02yk0f-ir-vl53l0x-laser-solutions-choose-distance-measurement-arduino-raspberrypi/#.XuUorJMzZp8){target=_blank}, [2](https://www.seeedstudio.com/blog/2019/12/23/distance-sensors-types-and-selection-guide/){target=_blank}, [3](https://www.learnrobotics.org/blog/ir-sensor-vs-ultrasonic-sensor/){target=_blank}.

## IMU

IMU is an abbreviation for the term Inertial Measurement Unit. Usually, this abbreviation is used for a device that combines accelerometer, gyroscope and magnetometer. In order to get a useful data from this sensor, in most cases it is required to implement concepts from the linear algebra and calculus. But depending on the libraries provided by the sensor manufactures and/or open source community, this could be already simplified a lot as so the IMU sensor can be used to measure the path driven by the vehicle and to control steering smoothness.

We provide links to three articles that are good start to understand how IMU can be used in a robotics projects: [1](https://maker.pro/arduino/tutorial/how-to-interface-arduino-and-the-mpu-6050-sensor){target=_blank}, [2](https://www.seeedstudio.com/blog/2020/01/17/what-is-imu-sensor-overview-with-arduino-usage-guide/){target=_blank}, [3](https://howtomechatronics.com/tutorials/arduino/arduino-and-mpu6050-accelerometer-and-gyroscope-tutorial/){target=_blank}.

## Power

Powering of the self-driving car for the robotics competition is not a trivial task. Depending on types of the motors and number of controllers, it could be found that separate power sources must be provided for different consumers.

The materials below should allow you better understand approaches that can be used to power the controllers and motors:

  * How to power the Arduino board: [1](https://thepihut.com/blogs/raspberry-pi-tutorials/how-do-i-power-my-arduino){target=_blank}, [2](https://technobyte.org/2016/07/power-up-the-arduino-uno/){target=_blank}
  * [How to power Raspberry Pi](https://thepihut.com/blogs/raspberry-pi-tutorials/how-do-i-power-my-raspberry-pi){target=_blank}
  * [Powering Motors with Arduino](https://learn.adafruit.com/adafruit-motor-shield-v2-for-arduino/powering-motors){target=_blank}
  * [Important note about the ground sharing](https://www.quora.com/Can-I-power-a-motor-driver-and-Arduino-with-separate-power-banks-I-am-using-two-12-V-motors){target=_blank}
  * [Usage a power bank with Arduino and Motors](https://www.quora.com/Can-we-use-Arduino-Uno-and-4-DC-motors-with-a-power-bank-to-power-a-Bluetooth-robot){target=_blank}
  * [Additional info to power the project](https://dronebotworkshop.com/powering-your-projects/){target=_blank}
