This is the readme file for Full_potential_steering_auto.py
        {5}                                          {4}
      _=====_                                      _=====_                                          |   [0], [1], [2], [3] ==> axes
     / _{6}_ \                                    / _{7}_ \                                         |   
   +.-'_____'-.----------------------------------.-'_____'-.+                                       |   {0}, {1}, ----------, {14}, {15} ==> button
  /   | [1] |  '.           X B O X            .'  |  _  |   \                                      |   
 / ___| /|\ |___ \                            / ___| {4} |___ \                                     |                         
/ |      |      | ;  __                  _   ; | _         _ | ;                                    |   
| | <---  --->[0] | |__|                |_|  | |{3}       {1}| |                                    |   NOTE : *********** Button Mapping Must Be Checked *********** 
| |___   |   ___| ;SELECT              START ; |___   _   ___| ;                                    |           
|\    | \|/ |    /            _____           \    | {0} |    /|                                    |   
| \   |_____|  .','"""""',    |___|  ,""[3]"', '.  |_____|  .' |                                    |   
|  '-.______.-' /   {12}  \  ANALOG /   /|\   \  '-._____.-'   |                                    |   
|              |{13}   {14}|-------|  <--|-->[2]               |                                    |   
|              /\   {15}  /         \   \|/   /\               |                                    |   
|             /  '._____.'           '._____.'  \              |                                    |   
|            /                                   \             |                                    |   
 \          /                                     \           /                                     |    
  \________/                                       \_________/                                      |   






Control Flowchart --->>>>
    .                                                            Tunrn ON
    .                                                               |
    .                        (Manual)                               |                             (Autonomous)       
    |-----------------------------------------------------------------------------------------------------------------------------------|
    |                      (By default)                                                 (To enter and exit press button {0})            |
    |                                                                                  (Inputs from joystick wil be disabled)           |
    |                                                                                                                                   |
    |                      (Basic Mode)                                                                        |                       \|/
    |--------------------------------------------------------|                                                 |    Things Will Be Done According to the value 
    |                      (By default)                      |                                                 |      publised on topic : "/motion" & "rot"
    |                                                        |                                                 |                        |
    |                                                       \|/                                                |                       \|/  
    |   Controls:                                                                                              |         "/motion" -> to get vel & omega
    |           button {1} --> Steering such that all wheels become perpendicular i.e. 90* _absolute_          |     "rot" -> to get the value of |self.rotin|
    |                          (threshold of 5* and cooldown of 10 sec)                                        |       |
    |                                                                                                          |       |---> |self.rotin| = 0 (by default)
    |           button {3} --> Steering for rotate in place motion i.e. FL = 55*     FR = -55*                 |       |        No Steering but moves acc. to "/motion"
    |                          (threshold of 5* and cooldown of 10 sec) BL = -55*    BR = 55* _absolute_       |       |
    |                                                                                                          |       |---> |self.rotin| = 1
    |           button {4} --> Steering such that all wheels become straight i.e. all become 0* _absolute_     |       |        Steering for rotate in place motion
    |                          (threshold of 5* and cooldown of 10 sec)                                        |       |        i.e.  FL = 55*     FR = -55*       
    |                                                                                                          |       |              BL = -55*    BR = 55*  _absolute_
    |           button {0} --> Autonomous button                                                               |       |        moves acc. to "/motion" only vel matter
    |                                                                                                          |       |
    |           button {7} --> Mode up button                                                                  |       |---> |self.rotin| = 2
    |                                                                                                          |       |        steering such that all wheels become 
    |           button {6} --> Mode down button                                                                |       |        straight i.e. all become 0*  _absolute_
    |
    |           axis [1]
    |              &       --> Steering and Driving like a RC car
    |           axis [2]
    |
    |           axis [3]   --> Steering for cuving motion i.e. FL = X*     FR = X*
    |                          (Real time)                     BL = -X*    BR = -X* 
    |                                                          
    |           axis [4]   --> Steer Mode (Full Press)
    |
    |           axis [5]   --> Full Potential Mode (Full Press)
    |
    |           NOTE : If button {3} is pressed then rover velocity will be controlled by axis [2]
    |
    |
    |
    |                         (Steer Mode)
    |--------------------------------------------------------|
    |            (To enter and exit full press axis [4])     |
    |                                                        |
    |                                                       \|/
    |   Controls:
    |           button {1} --> Steering such that all wheels turns 45* anticlockwise direction _relative_
    |                          (threshold of 5* and cooldown of 10 sec)
    | 
    |           button {4} --> Steering such that all wheels turns 45* anticlockwise direction _relative_
    |                          (threshold of 5* and cooldown of 10 sec)
    | 
    |           button {0} --> Autonomous button
    |
    |           button {7} --> Mode up button
    | 
    |           button {6} --> Mode down button
    | 
    |           axis [2]   --> Steering all the wheels in same direction i.e. FL = X*     FR = X*
    |                          (Real time)                                    BL = X*     BR = X*
    | 
    |           axis [3]   --> Steering for rotate in place motion i.e. FL = X*     FR = -X*
    |                          (Real time)                              BL = -X*     BR = X*
    | 
    |           axis [4]   --> Enter Basic Mode (Full Press)
    | 
    |           axis [5]   --> Full Potential Mode (Full Press)
    |
    |
    |
    |                     (Full Potential Mode)
    |--------------------------------------------------------|
    |            (To enter and exit full press axis [5])     |
    |                                                        |
    |                                                       \|/
    |   Controls:
    |           button {0} --> Autonomous button
    |
    |           button {7} --> Mode up button
    | 
    |           button {6} --> Mode down button
    | 
    |           axis [1]   --> Steering FL Wheel i.e. FL = X*
    |                          (Real time)
    | 
    |           axis [3]   --> Steering FR Wheel i.e. FR = X*
    |                          (Real time)
    | 
    |           axis [0]   --> Steering BL Wheel i.e. BL = X*
    |                          (Real time)
    |
    |           axis [2]   --> Steering BR Wheel i.e. BR = X*
    |                          (Real time)
    |
    |           axis [4]   --> Steer Mode (Full Press)
    | 
    |           axis [5]   --> Enter Basic Mode (Full Press)
