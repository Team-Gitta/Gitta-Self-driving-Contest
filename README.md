# 2020 Self-driving-Contest

실내 자율주행 응용 서비스 개발 공모전 Team Gitta입니다.

<화분 자동 관리 실내 자율주행차>

<br/>

# Table of Contents

- [System Configuration](#System-Configuration)
- [Self-driving Process](#Self-driving-Process)
- [QRcode Processing And Robot Arm Control](#QRcode-Processing-And-Robot-Arm-Control)

- [Gitta Web Interfate](#Gitta-Web-Interface)

<br/>

# System Configuration

<img width="100%" alt="hardware topology" src="https://user-images.githubusercontent.com/68395698/110611752-dca75080-81d2-11eb-8384-895c5988f400.png">
<img width="100%" alt="system configuration" src="https://user-images.githubusercontent.com/68395698/110611758-de711400-81d2-11eb-9bfc-d94dd86e9a7e.png">

<br/><br/>

# Self-driving Process

![selfdrivingprocess](https://user-images.githubusercontent.com/68395698/110628662-a378db80-81e6-11eb-9ef4-0d20de577e44.JPG)

<br/>

# QRcode Processing And Robot Arm Control

### Because the height, distance, and angle of the pot are automatically measured using the QR code, users only need to enter simple information about the pot in our websites

![cart](https://user-images.githubusercontent.com/68395698/110747095-29496500-8281-11eb-98ed-cba89a1a1eb9.png)

<br/>

## QR code detection

When the self-driving car arrives in front of the pot, it stops and starts detection.

[QRcode processing program](https://github.com/Team-Gitta/QRcode-Processing) in the raspberry pi detects QR code and decodes it.
The information in the Qrcode is the source of the pot.
It Checks that the value is equal to the id of the pot that needs to be watered.

If the id values match,
get coordinates of four points of the QR code.
Using the coordinates of the points, the angle, height, and distance of the pot are determined.

QRcode processing program in the raspberry pi locates the QRcode and draw blue lines using opencv

![pot](https://user-images.githubusercontent.com/68395698/110612901-fb5a1700-81d3-11eb-98bc-183ca669a86b.JPG)

<br/>

## Measuring the angle between pot and self-driving car

The angle of the pot and the self-driving car is determined using the position of the QR code point.

QRcode processing system divides the sections by angle and checks which section of the pot comes in.
After the decision is determined, the value is sent to the robot arm stm board using serial communication.

![angle1](https://user-images.githubusercontent.com/68395698/110614283-84be1900-81d5-11eb-8fe5-f3e75daa8d06.JPG)
![angle section](https://user-images.githubusercontent.com/68395698/110614433-acad7c80-81d5-11eb-82b3-239b60203021.JPG)

### [Differences in robot arm movement depending on the angle]

![angle2](https://user-images.githubusercontent.com/68395698/110615471-d024f700-81d6-11eb-8525-5f16ce677856.gif)
![angle2](https://user-images.githubusercontent.com/68395698/110615750-167a5600-81d7-11eb-8943-0977f80c1c64.gif)

<br/>

## Measuring the height of the pot

The height of the pot is measured using the point position at the top of the QR code.

QRcode processing program determines which section comes in according to the height of the pot and send it to the robot arm stm board.

![height](https://user-images.githubusercontent.com/68395698/110616143-91437100-81d7-11eb-87ea-a7df586a4a62.JPG)

### [Differences in robot arm movement depending on the height]

![height2](https://user-images.githubusercontent.com/68395698/110616151-930d3480-81d7-11eb-910f-b5c418569057.gif)
![height3](https://user-images.githubusercontent.com/68395698/110616155-94d6f800-81d7-11eb-9bce-15392d5fcf4a.gif)

<br/>

## Measuring the distance between pot and self-driving car

The length of four sides is measured using four points of the QR code.

Using this length, the distance between the pot and the self-driving car is determined.

Using the distance between the pot and the self-driving car, the robot arm decides how much the arm should stretch and sends the value to the stm board using serial communication.

<br/>

![distance](https://user-images.githubusercontent.com/68395698/110616615-16c72100-81d8-11eb-8430-ad7efe2d7993.JPG)

<br/>

## Water supply transmission

QRcode processing program brings registered feedwater information from database by matching the id of the Qrcode

It calculates the operation time of the water pump using the water supply information and send it to the stm board.

<br/>

## Robot arm and water pump operation

Using the values received by serial communication in the previous step on the stm board,
operate robot arms and water pumps.

After the robot arm and the water pump are finished, the robot arm will return to its initial state.
A message is issued in the robot wheel raspberry pi via mqtt to inform that the water supply operation is over.

<br/>

# Gitta Web Interface

### A website where users register their own pot information and generate QR codes

<br/>

## Register

The user enters the information of the plant to be registered. The entered information is stored in the database.

<br/>

## Result

When the user enters the pot information and clicks the Register button,
A QR code containing the unique id value of the pot is generated.

The user prints the generated QR code in size 5\*5 and attaches it to the top of the pot.

<br/>

## Mypage

The user can check the information of the pot registered.
