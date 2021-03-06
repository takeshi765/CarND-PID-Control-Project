# 1. Describe the effect each of the P, I, D components had in your implementation.

## (1) P

Kp contributes to how much steering angle the vehicle needs to use to correct the error between the goal (CTE=0) and the current state (CTE).  However, if this value is too big, the state will oscillate (underdamped) and can diverge. When I used the big value in this experiment, as I expected, CTE diverged, and the vehicle went off the road. On the contrary, when I used the small value, the steering angle was not enough to follow a curve, then the vehicle went off the road.

## (2) D

To alleviate the issue, Kd determines how much the system should suppress this steering angle based on the differential of CTE. If the change of CTE is big, this means that the velocity along the y-axis (assuming that the vehicle needs to drive along the x-axis in the vehicle's frame) is too big. Then, we do not want to use a big steering angle because the car can go to the opposite side of the road. Thus, we would like to reduce the steering angle if the velocity along the y-axis is significant. Kd controls this value. If Kd is too big, it will take a long time to converge (overdamped) or use the opposite steering angle although there is no oscillation. When I used a big value (relative to Kp) in this experiment, it took time to went back to the center of the road, or it used the opposite steering angle. When I used a small value (relative to Kp), the vehicle oscillated a lot and went off the road.

## (3) I

Ki contributes to correct the systematic bias. In particular cases, CTE never goes to zero because of the steering angle provided by Kp and Kd terms is not big enough to make CTE be zero. Then, if the sum of CTE becomes significant, that indicates that such situation occurs. Then, the vehicle will need to use a bigger steering angle. Ki controls this value. When I used a big value (relative to Kp and Kd), the vehicle oscillated. When I used a small value, I could not tell the difference. I assume that there may be minimal bias in the simulator.  If Kp, Kd, and Ki are properly tuned, the system will be critically damped where there is no oscillation, and the CTE quickly goes to zero (between underdamped and overdamped)

# 2. Describe how the final hyperparameters were chosen.
I used Twiddle leaned in the class to tune the gains. I used the parameters (Kd=0.2, Kp=3, Ki=0.004) from the class as the initial values. On the contrary to the course material, it was difficult to loop over multiple episodes using the simulator because it seemed that there was not "reset" message. I wrote a simple shell script using "xdotool" to fake mouse and keyboard input to restart the simulator automatically.  Each episode is 70 seconds. After 70 seconds, it evaluates the score and tunes the parameters based on Twiddle.  I did 383 episodes to tune the gains. The parameters gave the best score in the 43rd episode, and it never reached the best score after that.  The best parameters are Kp=0.21, Kd=3.16046, Ki=0.0039999. I stopped the training because dKp, dKd, and dKi became small enough.
