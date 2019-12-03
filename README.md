# Maze-Navigating-Robot

This repository is made with the intension to assist individuals with problems and concepts associated COE538 and should not be taken directly for lab work.

This Project was made for COE538, Microprocessors.
The code is comprised of assembly code.
The objective of this project is to allow an mobile eebot to traverse a course withour human intervention.
There are black guiding tape that will direct the eebot on its path towards the exit. However, many of these black tracks lead to deadends (walls).

The eebot generally consist of 2 motors, a LCD panel, led sensors in the formation of a star, and a micro controller.
General Formation of Sensors:

ROW 1:     .A   
ROW 2: B.  .C  .D     
ROW 3:   E. .F

In the Code, sensors are called under alternate names.
-  A - SENSOR_BOW
-  B - SENSOR_PORT
-  C - SENSOR_MID
-  D - SENSOR_STBD
-  E-F - SENSOR_LINE 

 - These sensor photoresistors which display high resistance upon seeing darkness (ie. black guide line of the course) and low resistance when illuminated. (ie white background.)
 - E and F are combined sensors, displaying a shared value depending on which Sensor is detecting a change in light.
 
 Motors are operated through STAR and PORT subroutines.
 
 1.Finding Thresholds:
 - Since resistance valuse change upon entering a dark or light source, we must test the sensors for their spikes in resistance. This is achieved through passing sensors A, and D from the white line to the black line. These are our desired sensors for our turning logic.
 - E-F must bet aligned on the black guide tape, while shifting the bot left and right to see the change in the resistance values.
 - From these drastic changes in resistance, a HEX value from the displayed LCD will be chosen as the min/max valueto be compared to when deciding on a turn.

2. Line Following Stability
- Not all turns will be smooth. When test in an common ground enviroment, all test are subject to variable change. Thus, we can not assume our bot will make perfect turns or walk perfectly straight since motors can be faulty, the course can be dusty, or the sensors just don't work. To ensure the bot can navigate curves and correct its orientation. We need SENSOR_LINE to detect whether the bot is misaligned.
- Thus, using previously obtained threshold values, the E-F readings will trigger the bot to make slight turn adjeustments as the E - F sensor detects a change.

 3. Turns:
 - The for our code, we decided to make the bot right turn dominant. This means we prioritize only turns that require the bot to make a right turn. As such, we look to SENSOR_STBD for it's changes in resistance. If there is a spike (ie. black tape is seen), the bot will turn right until SENSOR_BOW (A) is detecting the black tape. This allow the bot to continously move forward while making right turns when needed.
 - However, what if the right turn leads to a wall (i)? Or there is a straight path with a left turn needed to pass the course (ii)?
    1. The front bumper will trigger upon hitting the wall and make the reverse slightly, make a 180 degress turn, and continue forward.
    This is achieved by making the bot auto turn for a small intervale in such that SENSOR_STDB (D) is off the black guide line, to     continue to turn until SENSOR_BOW hits the black tape again. We choose D instead of A as the stopping trigger to ensure E - F are facing the black line as much as possible to allow  the bot to adjust themselves.
    2. When there is a stright path and a left turn is required, this means the bot will eventually hit another wall. When it hits the wall, the right turn function will be on the correct side of the black guide and make a correct turn (after its 180 degrees turn).

EX. |- - - - -        <--- [Entering]                                                                                                                   
 ------->    |           After 180 degrees turn, detects new right turn line
 
 .................|
 
 .................|
 [Finishing]
                                                                                    
4. State Machine
- The state machine consisted of eight states, ALL_STOP, START, FWD, PAUSE, LEFT_ADJ, RIGHT_ADJ, RIGHT_INT, REV. Initially, the state machine is in the ALL_STOP state where pressing the front bumper will change the control state to START, and letting go of the front bumper will change the state to FWD.
- The FWD state makes the eebot move forward. This state can branch to multiple other states depending on which conditions are met. If no conditions are met, the state will automatically change to the PAUSE state. 
- The PAUSE state makes the eebot stop any movement by turning off the motors. During this state, if the starboard sensor (D) passes a certain threshold by reading the dark line, the state will change to RIGHT_INT. However, if no conditions are met the state will automatically change back to the FWD state.  
- The ALL_STOP state will stop the motors just like the PAUSE state. The difference is that the ALL_STOP state is activated when the back bumper is pressed during any state except PAUSE and START. It also does not automatically move to a new state until the front bumper is pressed once again.
- The LEFT_ADJ state makes the eebot move slightly in the left direction. It activates during the FWD state if the line sensor (E-F) passes the lower threshold which detects that the eebot is not aligned with the guide path and must make a left turn adjustment to stay on course. After the adjustment, the state changes to PAUSE. 
- The RIGHT_ADJ state functions identically to the LEFT_ADJ state except it makes the eebot move slightly in the right direction, and it activates if the line sensor passes the upper threshold. After the adjustment, the state will also change to PAUSE.
- The RIGHT_INT state makes the eebot take a right turn at the right intersection. This activates when the starboard sensor passes a certain threshold, and will change to a PAUSE state once the bow’s sensor (A) detects it is back on the dark line via another threshold.
- The REV state causes the eebot to make a 180° turn. This happens when the front bumper is pressed while in the FWD state. The eebot will back up and then turn until the starboard sensor passes a certain threshold which will allow the eebot to be aligned with the dark line again.

[This concludes the explaination of the code and methodology of the design]
