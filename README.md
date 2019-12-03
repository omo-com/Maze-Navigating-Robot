# Maze-Navigating-Robot

This repository is made with the intension to assist individuals with problems and concepts associated COE318 and should not be taken directly for lab work.

This Project was made for COE538, Microprocessors.
The code is comprised of assembly code.
The objective of this project is to allow an mobile eebot to traverse a course withour human intervention.
There are black guiding tape that will direct the eebot on its path towards the exit. However, many of these black tracks lead to deadends (walls).

The eebot generally consist of 2 motors, a LCD panel, led sensors in the formation of a star, and a micro controller.
General Formation of Sensors
       
       .A     
   B.  .C .D    
     E. .F

In the Code, sensors are called under alternate names.
 A - SENSOR_BOW
 B - SENSOR_PORT
 C - SENSOR_MID
 D - SENSOR_STBD
 E-F - SENSOR_LINE (E and F are combined sensors.)
 Motors are operated through STAR and PORT.
 
 1.Finding Thresholds:
 
                                                                                                
