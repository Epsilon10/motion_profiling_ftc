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

We now need to reparametrize all the segments into a single variable of arclength ($$r(t) \in [0, 1] \to s$$ )

$s$ is simply the displacement along the curve.

thus: $$|r'(s)| = 1 = \sqrt{(\frac{dx}{ds})^2 + (\frac{dy}{ds})^2}$$as 1 unit 

It is simple to represent $s(t)$ as arclength as a function of time
$$
s(t) = \int^{t}_0 |r'(\tau)|\, d\tau = \int^{t}_0 \sqrt{(\frac{dx}{d\tau})^2 + (\frac{dy}{d\tau})^2} \, d\tau
$$
We can numerically integrate this and much resulting arc lengths to times



