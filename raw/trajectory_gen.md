*Trajectory Generation** 

**Quintic Spline Trajectories**

Our splines will be expressed as a vector of single variable functions for the x and y component.

Quintic splines consist of a series of segments combined into a single peicewise curve. Each segment is a parametric curve with a quintic polynomial for each component. 

For most of our segments in FTC, we will use one segment, which will also allow for some more brevity in our notation.

$$r(t) = \begin{cases}
x(t) = a_xt^5 + b_xt^4 + c_xt^3 + d_xt^2 + e_xt + f_x , \\
y(t) = a_yt^5 + b_yt^4 + c_yt^3 + d_yt^2 + e_yt + f_y
\end{cases}$$

where $0 \le t \le 1$

**Reparametrization to Arc Length**

We now need to reparametrize all the segments into a single variable of arclength ($$t \in [0, 1] \to s$$ )

$s$ is simply the displacement along the curve.

thus: $$|r'(s)| = 1 = \sqrt{(\frac{dx}{ds})^2 + (\frac{dy}{ds})^2}$$as 1 unit 

It is simple to represent $s(t)$ as arclength as a function of t
$$
s(t) = \int^{t}_0 |r'(\tau)|\, d\tau = \int^{t}_0 \sqrt{\left(\frac{dx}{d\tau}\right)^2 + \left(\frac{dy}{d\tau}\right)^2} \, d\tau
$$
We can numerically integrate this and match resulting arc lengths to times

We can now determine $r(s)$ by composition: $r(t(s))$

We can now differentiate to yield the parametrized derivative:
$$
r'(s) = r'(t) \cdot t'(s) = \frac{r'(t)}{s'(t)} = \frac{r'(t)}{|r'(t)|}
$$
We realize this makes sense as we can realize that $r'(s)$ = 1.

Now let us set $s(t)$ be the state of the motion profile at time $t$.

We now have
$$
v(t) = \frac{d}{dt}[r(s(t))] = r'(s(t))s'(t)
$$
**Heading**

In addition to the translational motion, we also need to interpolate some type of angular motion. 

For nonholonomic drive trains, we can update the heading like so:

$$\theta(t) = \arctan \frac{y'(t)}{x'(t)}$$

For holonomic drive trains, we can consider the heading component to be a completely independent spline. 

$$\theta'(t) = \dfrac{x'(t)y''(t) - x''(t)y'(t)}{x'(t)^2 + y'(t)^2}$$

We can now calculate this derivative at $t=0$ and $t=1$



**Generating a Profile**

We now want to generate a motion profile $s(t)$ = arc length traveled where t is time

Satisfying maximum velocity, acceleration, and angular velocity constraints.

**Forward Pass**

We look at pairs of consecutive states, where the start state has already had its velocity determined. We now need to find an acceleration that is allowable at both the start and end state

**Steps**

- Compute the segment constraints (max vel, max acc)

- If the last velocity exceeds the max velocity, we just coast at max velocity

- Else, we can compute the final velocity assuming max acceleration 
  $$
  v_f = \sqrt{v^2 - 2a_{max}\Delta s}
  $$

- If this final velocity is under the maximum velocity, we are good and we can continue accelerating

- Otherwise we want to split the segment:
  $$
  \Delta s_{acc} = \dfrac{v_{max}^2 - v^2}{2 a_{max}}
  $$


- We can then deaccelerate for $\Delta s_{acc}$, and then coast when we reach max velocity

- We now repeate this process starting at the last planning point and going back

- Now we want to obtain our final states, so we now need to compare forward and backward state deltas

- If there is a difference in the displacements of the state, we can split the longer segment into two and add the second segment to our forward state

- Lets define some terms:

  $\Delta s_{f}$ , $\Delta s_{b}$ are the forward and backward displacements respectively

  case $\Delta s_f > \Delta s_b$:

  ​	We calculate the kinematic state after traveling $\Delta s_f - \Delta s_b$

  case $ \Delta s_f < \Delta s_b $

  ​	We calculate the kinematic state after traveling $\Delta s_b - \Delta s_f$



  we add the resulting kinematic state to its respective state list (forward or backward).

- After we establish forward and backwards consistency, we now need to calculate the end state after the alginement

- We then calculate the minimum velocities at each start and end state... and if the backward state has a lower velocity than the forward state, we calculate the intersection between the two end states and add that to our final states, by using the following equation:
  $$
  \dfrac{v_{prev}^2 - v^2}{4a_t a_{{prev}_{t}}}
  $$


- Finally we want to turn the final states into time parametrized motion segments

  $v$ = state velocity
  $$
  \Delta t = -\frac{v}{a_t} \pm \sqrt{\frac{v^2}{a^2_t} + \frac{2\Delta s}{a_t}}
  $$
  Note: in order to do this, we need to keep track of the displacements for each state

- We now can generate a motion profile containing in which each state satisfies velocity and acceleration constraints

**References**

- https://github.com/acmerobotics/road-runner/blob/master/doc/pdf/Quintic_Splines_for_FTC.pdf 

- http://www2.informatik.uni-freiburg.de/~lau/students/Sprunk2008.pdf