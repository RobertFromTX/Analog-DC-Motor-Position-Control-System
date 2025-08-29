# Analog PI Control for Basic Positioning of a Brushed DC Motor

## Video Demo
https://github.com/user-attachments/assets/2bba8920-0b15-401e-9de0-5b88466c60b8

If the video does not play, refresh the page.

## Project Overview and Goals
This is a project I completed at the beginning of Summer 2025. Essentially it turns a brushed DC motor into a servo motor, though it cannot produce the torque of one. Given an input voltage in the range of 0V-5V, it positions the motor to an angle proportional to the input voltage. For example, 0V would position the motor at 0 degrees while 2.5V would position the motor at 180 degrees. The controller is built with op-amps and an encoder. Initially, I wanted to create a PID controller, but the signals from the derivative term were often too noisy. As a result, I ended up with a PI controller. Nevertheless, a PI controller is more than sufficient to control the position of the motor.

![Motor Controller](images/IMG_3965 .JPG)

## Design
There were two design constraints that I set for this project. The first design constraint was that I could only use parts I already had on hand. The second design constraint was that only one breadboard can be used to place the circuit components onto.

### Preliminary Design
The first step taken in the design phase was to plan how the overall structure of the system should be to accomplish the control task. The overall system requires three components: an actuator, a sensor, and a PI Controller. The sensor's task is to determine the angular position of the motor, which the PI controller will use to correct the position of the motor. Below is a block diagram of the preliminary design. <br>
![Preliminary Design Diagram](<images/Preliminary Design.png>)

### Detailed Design
After the components of the system have been identified, the next step is to create the actual designs of each component.

**Motor:**<br>
For the motor, a simple brushed DC motor was chosen.

**Sensor:**<br>
For the sensor, the AS5600 breakout board was chosen. 

**PI Controller:**<br>
Designing the PI Controller was easily the most difficult part of this project. The approach that I used was to break the PI Controller down into its sub-components and test each individually. In total I identified 5 sub-components as I progressed through the PI controller:

1. Subtraction op-amp circuit
2. Proportional op-amp circuit
3. Lossy Integrator op-amp circuits
    * why? To prevent integral windup.
4. Summing op-amp circuit
5. Buffer to drive DC motor

Before I finalized my component values for each op-amp configuration, I ran simulations using LTspice to ensure that the circuits behaved desireably. Then, I created a model in simulink to ensure that I can correctly integrate each component together. Below is my Simulink block diagram.
![Simulink Block Diagram](<images/Simulink Example.png>) <br>
Now that I determined that the system can be integrated properly, I designed a circuit by selecting passive component values and creating a schematic in Kicad. Below is a schematic of the final circuit.
![KiCad Schematic](<images/KiCad Schematic.png>)
## Results and Takeaways
In the end, I basically ended up with a servo motor. The system did behave how I desired, but I know that there are not many use cases for it besides educational purposes since the controller is quite heavy while the motor produces very little torque. Other brushed DC motors can be substituted easily, which is a perk of my controller.

I found the control problem to be quite easy. Unlike a drone or a self-balancing robot, this system is not inherently instable. As a result, there is not much tuning required to get a minimally functional system. On the other hand, the circuitry was quite a challenge. There were tons of roadblocks when it came to signal integrity. There were many things creating undesirable noise in my system, making it not work at all. Small things such as having wires near my system would generate a lot of noise, and I had to use my oscilloscope on multiple occassions to finally get my system working. As a result, I decided that my next few projects would be based on microcontrollers since it is much easier to create reliable systems that are based on microcontrollers.
