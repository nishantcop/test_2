[only_p]: ./image/only_P.gif "only_P"
[only_p_half]: ./image/only_P_half.gif "only_P"
[only_I]: ./image/only_I.gif "only_I"
[only_I_half]: ./image/only_I_half.gif "only_I"
[only_D]: ./image/only_D.gif "only_D"
[only_D_half]: ./image/only_D_half.gif "only_D"
[final_half]: ./image/final_half.gif "only_D"
# CarND-Controls-PID
PID stands for Proportional-Integral-Derivative. The goal of this project is to create an amalgamation of these controllers to generate controlled signal to throttle, brake, steer such that it can help drive a car in real world. 

For this project we will be using race track (simulator provided in [project intro page]((https://github.com/udacity/self-driving-car-sim/releases)) in the classroom). The simulator will provide cross track error (CTE) and speed that helps us to derive proper signals.

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

NOTE: Additionally, I've also confiigured this project to build using Visual Studio on Windows platform with uWebSockets using instructions listed [here](http://www.codza.com/blog/udacity-uws-in-visualstudio)

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

### Describe the effect each of the P, I, D components had in your implementation

#### Proportional (P) component: 
This controller is responsible for steering the car towards the center in proportion to CTE with a proportional factor tau.
```
-tau * cte
```
If I use P controller alone [`pid.Init(1, 0.0, 0.0);`] will see the oscillation (from the center of the lane) behavior of the car. This keep increasing over a period of time and car quickly goes out of the track. 

![only_p]

#### Integral (I) component: 
The I controller is responsible to deal with systematic bias by summing all errors. This could be possible the error will go away due to the bias which causes `P` and `D` to never stablize to center. The bias could cause steering drift, etc.
```
int_cte += cte
tau_i * int_cte
```
When I used `I` controller alone [`pid.Init(0.0, 1.0, 0.0);`], it quickly steered the car towards the left. 

![only_I]

#### Differential (D) component: 
This controller takes care of the temporal derivative of error. What effectively it does, it tries to control the steering such that once the car has turned enough to reduce the error and ensures it doesn't overshoot center line. 

```
diff_cte = cte - prev_cte
prev_cte = cte
- tau_d * diff_cte
```
When I used `D` controller alone [`pid.Init(0.0, 0.0, 1.0);`], the car slowly steered towards the right. 

![only_D]

![only_P]
![only_I]
![only_D]

![only_P_half]

![only_I_half]

![only_D_half]

![final_half]

C:\Users\nrana\Downloads\only_P.gif

