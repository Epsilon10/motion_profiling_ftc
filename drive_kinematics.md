**DRIVE KINEMATICS**

**Drive update**

- get encoder values of all wheel positions

- do (change in wheel positions)/(change in time) for each wheel to determine the wheel velocity

- **Tank Kinematics**

  x pointing forward, y pointing left

  We have 

  $$ S_L, S_R$$ = left distance and right distance

  $$r$$ = turn radius from inner wheel

  $$b$$ = track width

  $$\theta$$ is turn in radians

  $$S_L = r \theta$$

  $$S_R = (r + b)\theta$$

  $$S_M = (r + b/2)\theta$$

  We are now interested in how the x and y coordinates and heading change with respect to time.

  $$\frac{dx}{dt} = m(t) \cos(\theta(t))$$

  $$\frac{dy}{dt} = m(t) \sin(\theta(t))$$

  **Derivation**

  Frame of reference: center point of left wheel

  We have the following equation:

  $$\frac{d\theta}{dt} = \frac{(V_R - V_L)}{b}$$

  Integrating and setting $$\theta(0) = \theta_0$$ we find a function for calculating the robot's orientation as a function of wheel velocity and time:

  $$\theta(t) = \frac{(V_R - V_L)t}{b} + \theta_0$$

  The velocity of the robot is the average of tHe two wheels: $$ \frac{(V_R - V_L)}{2}$$

  We now have the following differential equations:

  $$\frac{dx}{dt} = \frac{(V_R - V_L)}{2} \cos(\theta(t))$$

  $$\frac{dy}{dt} = \frac{(V_R - V_L)}{2} \sin{\theta(t)}$$



  **Mecanum Kinematics**

  Each wheel has an additional x and y velocity component due to the vehicles rotational velocity $$\omega_v$$ given by

  $$V_xr = -Y_n \cdot \omega_v$$

  $$V_yr = X_n \cdot \omega_v$$

  The total linear velocity given by the vector componenets is

  $$V_{x_n} = V_x + V_{x_{r_n}} = V_x - Y_n \cdot \omega v$$

  $$V_{y_n} = V_y + V_{y_{r_n}} = V_y - X_n \cdot \omega v$$

  $$\omega = \frac{V_x + V_y \cdot \tan{\theta}}{r}$$ = wheel velocity



  **Odometry**

  We have our target velocity as

  $$v(t) = (V_x \cos (\theta_0 + \omega\tau) - v_y \sin(\theta_0 + \omega\tau),  V_x \sin (\theta_0 + \omega\tau) + v_y cos(\theta_0 + \omega\tau)$$

  Integrating results in
$$
x(t) = \int^{\Delta t}_0 v(\tau) \, d\tau = (\frac{v_x}{\omega}[\sin \theta_1 - \theta_0] + \frac{v_y}{\omega}[\cos \theta_1 - \cos \theta_0]), (-\frac{v_x}{\omega}[\cos \theta_1 - \cos \theta] + \frac{v_y}{\omega}[\sin \theta_1 - \sin \theta_0])
$$

  our updated robot pose (x and y component)



