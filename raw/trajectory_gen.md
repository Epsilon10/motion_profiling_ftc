**Trajectory Generation** 

Thanks Ryan Brott: https://github.com/acmerobotics/road-runner/blob/master/doc/pdf/Quintic_Splines_for_FTC.pdf 

For helping me understand this topic

As well as 

http://www2.informatik.uni-freiburg.de/~lau/students/Sprunk2008.pdf

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

It is simple to represent $s(t)$ as arclength as a function of time
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

