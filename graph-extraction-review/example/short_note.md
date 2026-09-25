# The Harmonic Oscillator

Consider a one-dimensional harmonic oscillator with mass \(m\), angular frequency \(\omega\), and Hamiltonian

\[
H = \frac{p^2}{2m} + \frac{1}{2}m\omega^2 x^2.
\]

The canonical equations give \(\dot{x}=p/m\) and \(\dot{p}=-m\omega^2x\). Eliminating \(p\) yields

\[
\ddot{x}+\omega^2x=0.
\]

For initial conditions \(x(0)=x_0\) and \(p(0)=p_0\), the solution is

\[
x(t)=x_0\cos(\omega t)+\frac{p_0}{m\omega}\sin(\omega t).
\]

The energy \(E=p^2/(2m)+m\omega^2x^2/2\) is constant because
\[
\frac{dE}{dt}=\frac{p}{m}\dot p+m\omega^2x\dot x=0.
\]

This argument assumes \(m>0\), constant \(m\) and \(\omega\), and differentiable trajectories. It does not describe damping, driving, nonlinear forces, or quantum-mechanical commutators.
