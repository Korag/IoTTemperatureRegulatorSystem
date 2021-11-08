# IoTTemperatureRegulatorSystem

IoTTemperatureRegulatorSystem is a project of a complex system of temperature regulation service including both hardware and software layer.

The temperature control system, due to its degree of dispersion of physical elements in space, is divided into several smaller independent modules performing the set functions:

1. A temperature sensor chip that sends its measurements to the controller:
⋅⋅⋅a) ESP32.⋅⋅
⋅⋅⋅b) DS18B20 temperature sensor - digital 1-wire THT.⋅⋅
⋅⋅⋅c) Communication via MQTT and WiFi.⋅⋅
2. A hardware actuator circuit in the form of a thermal fan (simulated by an LED) that receives control signals from the controller:
⋅⋅⋅a) ESP8266.⋅⋅
⋅⋅⋅b) LED.⋅⋅
⋅⋅⋅c) 270 Ω resistor.⋅⋅
⋅⋅⋅d) Communication via MQTT and WiFi.⋅⋅
3. Mongo Atlas database storing recorded temperature measurements and temperature control settings.
4. MQTT broker based on mosquitto. The broker is installed on a virtual machine running CentOS on Azure VM and has a static IP address.
5. A web application to observe the current status of the temperature control system and to edit the preset settings. Developed in React.js technology together with ASP.NET Core Web Api. The application is hosted in Azure App Service.
6. A program controlling (driver) the whole system, which is responsible for receiving measurements sent by the temperature sensor system using the MQTT protocol. It places the received measurements in a database and then takes appropriate actions based on the last measured value and the set temperature control settings. The controller has a possibility to control the actuator system in a form of a thermo fan through the MQTT protocol - it sets the on/off state and 1 of 4 user preset heating power levels. The application is written using ASP.NET Core technology and is a console application that runs in an infinite loop. The application is hosted as a WebJob in the Azure App Service.

[Video presentation (in Polish)](https://drive.google.com/file/d/1y3x3fiZzYx_mAQ-jG-lYRKBAApaJIm5i/view?usp=sharing)

Diagram of the relationship between the components of a temperature control system:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_1.PNG "Components diagram")

Realised temperature sensor circuit based on ESP32:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_2.PNG "Temperature sensor circuit")

A realised hardware actuator circuit in the form of a thermo fan which is simulated by the LED used:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_3.PNG "Hardware actuator circuit")

App login page:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_4.PNG "Login page")

Client application launched, which includes a section for setting parameters and a section for visualising the current state of the system:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_5.PNG "App main page")

Test scenario: we artificially cool down the temperature in the test room - the sensor sends out increasingly lower temperature readings. The temperature value has already exceeded the hysteresis threshold, but the LED still remains off:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_6.PNG "Test scenario")

Test scenario: the value of the last temperature readings fell below the setpoint, causing the heating system to start (LED on). Heating will be on until the temperature is greater than the hysteresis setpoint:

![alt text](https://github.com/Korag/DocumentationImages/blob/master/IoTTemperatureRegulatorSystem/IoTTemperatureRegulatorSystem_7.PNG "Test scenario")
