# Brushless-Motor-Driver-QS-910


# The Main Idea:

* Connect the Brushless motor driver for the robot base 

# Components:

1. Arduino
2. Battery
3. QS-910 
4. Brushless motor
5. Potentiometer x2
6. Switch

## QS-910:

![Screenshot_2](https://user-images.githubusercontent.com/85455361/128207243-51a803b3-19b3-4222-bbb5-a3185b148eb0.jpg)


### As shown. it has 5 pins on each side:
1. Right Pins:

    ![Screenshot_3](https://user-images.githubusercontent.com/85455361/128218201-235e4930-5a46-429e-bacf-8433091b8640.jpg)
    
    1. Vcc (5V)
    2. Signal pin or what we call it PWM; we can use it to control the speed
    3. Z/F pin which is responsible for the direction of the wheel 
    4. VR or voltage controller pin; we can use it to control the speed too
    5. GND pin


2. Left Pins
    
    ![Screenshot_8](https://user-images.githubusercontent.com/85455361/128218426-bf6f0ab8-5510-4188-b66a-d0e1c4570fe0.jpg)

    
    1. MA/U ; coming from the brushless motor (phase 1)
    2. MB/V ; coming from the brushless motor (phase 2)
    3. MC/W ; coming from the brushless motor (phase 3)
    4. GND 
    5. VCC of the battery 

## Brushless motor ( wheel )

![Screenshot_6](https://user-images.githubusercontent.com/85455361/128208710-5a3bd462-0a1d-4b7e-8416-3858e381f8c7.jpg)

### There are three importan wires coming from the motor :

1. Yellow is MA
2. Green is MB
3. Blue is MC 

# Connection:

![Circuit](https://user-images.githubusercontent.com/85455361/128220442-72357135-b3e9-4528-b7a7-227a173ab0c0.jpg)

# Code 

* defining:
``` c++  
  bool WheelDirection = 2;  //Z/f pin
  int PWM_WheelSpeed = 3;   //Singal pin (0-1023)
  int Voltage_WheelSpeed = 5; //VR pin  (0v-5v)

  int PotPWM = A0;
  int PotVoltage = A1;
  int PWM;
  int VoltageRange;

  bool switchDirection = 4; //Switch for changing the direction
  bool switchDirectionStatus;

``` 


* Setup:

``` c++
  pinMode(WheelDirection, OUTPUT);
  pinMode(PWM_WheelSpeed, OUTPUT);
  pinMode(Voltage_WheelSpeed, OUTPUT);

  pinMode(PotPWM, INPUT);
  pinMode(PotVoltage, INPUT);

  pinMode(switchDirection, INPUT);

  Serial.begin(9600);
```

* The main body (points):
  
For reading from the potentiometers and switch:
``` c++
  PWM = map(analogRead(PotPWM), 0, 1023, 0, 100); // the speed of PWM would be from (0-100)
  VoltageRange = map(analogRead(PotVoltage), 0, 1023, 0, 5); // the speed of VoltageRange would be from (0v-5v)

  switchDirectionStatus = digitalRead(switchDirection);
```
We have two states for the switch: 

1. If the switch is in LOW status:
``` c++
  if ( switchDirectionStatus == 0) //Backward
  {
    digitalWrite(WheelDirection, LOW);
    analogWrite(PWM_WheelSpeed, PWM);
    delay(100);
    analogWrite(Voltage_WheelSpeed, VoltageRange);
    delay(1000);
  }
```
2. If the switch is HIGH status: 
``` c++
  if ( switchDirectionStatus == 1) //Forward
  {
    digitalWrite(WheelDirection, HIGH);
    analogWrite(PWM_WheelSpeed, PWM);
    delay(100);
    analogWrite(Voltage_WheelSpeed, VoltageRange);
    delay(1000);
  }

``` 
### Note:

1. i have two ways for controlling the speed:
    1. By the PWM 
    2. By changing the voltage

2. We have two ways for controlling the speed but in our case using the Signal pin (PWM) is usefull while in case where there is no controller (Arduino) we use the VR pin because we will use an external voltage (battery or power supply).

## Important Notes:
* I can not simulate the circuit because the driver is not available on the simulation platform
