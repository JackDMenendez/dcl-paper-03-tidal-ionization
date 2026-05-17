# Quantum Roche limit -- M_min(d) derivation outline

**Status:** DRAFT (synthesises Paper~I follow-on catalogue entry~#9
into the analytical form that lands in Paper~III's main body).
**Purpose:** Translate the gradient-ionization condition of
`notes/plasma_as_gravitational_ionization.md` (inherited from
Paper~I) into the explicit $M_\text{min}(d)$ form, identify the
$d^3$ scaling as the quantum-mechanical analogue of the classical
Roche limit, and outline the paper-section derivation.
**Cited by:** the audit-table rows "Quantum Roche limit
$M_\text{min}(d) \cdot R_1 / d^3 = \Delta\omega_\text{tongue}$"
and "$d^3$ scaling is the quantum-mechanical analogue of the
classical Roche limit" in `paper/sections/audit_table.tex` (both
\texttt{STUB} in v0.1; this note is the source-of-truth for the
derivation that flips them to \texttt{PASS}).

---

## Starting point

From `notes/plasma_as_gravitational_ionization.md` (Paper~I), the
gradient-ionization condition for hydrogen at the ground state $n=1$
is

$$\left| \frac{\nabla\rho_\text{clock}}{\rho_\text{clock}} \right| \cdot R_1 \;>\; \Delta\omega_\text{tongue},$$

where $R_1$ is the Bohr radius (lattice units) and
$\Delta\omega_\text{tongue}$ is the Arnold-tongue width of the
$n=1$ resonance.  This is a condition on the *fractional* clock-
density gradient across the atom.

## External mass at distance $d$

A point mass $M$ at distance $d$ from the atom (and far compared to
$R_1$, so a multipole expansion converges) imposes a clock-density
field with magnitude

$$\rho_\text{clock}(r) \;\sim\; \bar{\rho} \,\bigl(1 + \tfrac{M}{r}\bigr)$$

at leading order in the Newtonian-gravity limit
(see Paper~I §7 for the clock-density-of-gravity identification).
The gradient at distance $r$ is

$$|\nabla\rho_\text{clock}| \;\sim\; \frac{M}{r^2}.$$

The *fractional* gradient across the atom, evaluated at $r = d$, is

$$\left| \frac{\nabla\rho_\text{clock}}{\rho_\text{clock}} \right|_{r=d} \cdot R_1 \;\sim\; \frac{M}{d^2} \cdot R_1.$$

But this is the gradient at a single point, not the *differential*
gradient across the atom of size $R_1$.  The differential gradient
-- the change in $|\nabla\rho|/\rho$ across $R_1$ -- is one
derivative further, giving an extra factor of $1/d$:

$$\Delta\left( \frac{|\nabla\rho_\text{clock}|}{\rho_\text{clock}} \right)_{R_1} \cdot R_1 \;\sim\; \frac{M \cdot R_1}{d^3}.$$

This $d^{-3}$ scaling is the same as the classical Roche limit and
is not accidental: it comes from the standard tidal-force
multipole expansion (force $\sim M/r^2$, force *difference* across
a body $\sim M/r^3 \cdot \ell$ for a body of size $\ell$).

## The threshold condition: $M_\text{min}(d)$

Substituting the tidal-gradient estimate into the
gradient-ionization condition gives the threshold

$$\boxed{\;M_\text{min}(d) \;=\; \frac{\Delta\omega_\text{tongue} \cdot d^3}{R_1}.\;}$$

This is the **quantum Roche limit**: the minimum external mass at
distance $d$ that suffices to disrupt the $n=1$ Arnold-tongue
lock-in via gradient detuning, independent of temperature.  The
$d^3$ scaling identifies it as the quantum-mechanical analogue of
the classical Roche tidal-disruption radius, with
$\Delta\omega_\text{tongue}$ replacing the classical body density.

## Why the $d^3$ scaling matches classical Roche

The classical Roche limit reads

$$d_\text{classical} \;=\; r_\text{primary} \cdot \bigl( 2 \rho_\text{primary} / \rho_\text{satellite} \bigr)^{1/3},$$

i.e. $d^3 \propto M / \rho_\text{satellite}$, where the satellite's
self-gravity (density $\rho_\text{satellite}$) is the restoring
mechanism overcome by the tide.  The quantum case replaces the
satellite's self-gravity restoring force with the Arnold-tongue
restoring force (basin width $\Delta\omega_\text{tongue}$).  Both
give $d^3 \propto M / \text{(restoring strength)}$; both come from
the standard tidal-multipole expansion (force gradient $\sim
M/d^3$).  Same mathematical structure, derived from opposite
directions:

- *Classical Roche:* Newtonian tidal force overcomes self-gravity.
- *Quantum Roche:* tidal clock-density gradient overcomes the
  Arnold-tongue basin width.

In the lattice framework they are the same thing.

## Numerical match to `exp_18`

`src/experiments/exp_18_tidal_ionization.py` applies a *linear*
gradient $g \equiv |\nabla\rho_\text{clock}| / \rho_\text{clock}$
directly, rather than computing it from an external $M/d^3$.  The
correspondence is

$$g \;\equiv\; \frac{M}{d^3}\quad\Longrightarrow\quad g_\text{crit} \;=\; \frac{\Delta\omega_\text{tongue}}{R_1}.$$

Paper~I's free-particle tongue-width estimate
$\Delta\omega_\text{free} \approx 0.166$ and $R_1 = 10.3$ give the
prediction

$$g_\text{crit} \;\approx\; 0.016\ \text{per node}.$$

`exp_18`'s g-scan covers $g \in \{0, 0.004, 0.008, 0.012, 0.016,
0.040, 0.080\}$ -- the third-from-last value sits at the predicted
critical gradient.  The numerical confirmation is that
$T_\text{escape}(g)$ should drop sharply between $g = 0.012$ and
$g = 0.040$, with the sharpest fall near $g_\text{crit} \approx
0.016$.

## What the paper-text derivation needs to add

The derivation here is a back-of-envelope ($\sim$) argument; the
paper-text version should:

1. **Replace the proportionality with an equality.**  Identify the
   numerical prefactor in the $M$-to-$g$ conversion.  At order of
   magnitude this is unity; for a quantitative match to observation,
   the prefactor matters.
2. **Use the actual radial profile of $\rho_\text{clock}(r)$.**
   Paper~I §7 gives $\rho_\text{clock}(r) = \bar{\rho} \cdot
   \exp(-\phi(r) / c^2)$ in the weak-field limit, not the linear
   approximation.  The $d^3$ scaling survives the exponential
   correction at leading order; quantify the deviation.
3. **Identify the $\Delta\omega_\text{tongue}$ value at the
   actual hydrogen $n=1$ resonance.**  Paper~I's
   $\Delta\omega_\text{free} \approx 0.166$ is the free-particle
   tongue width; the $n=1$ lock-in inherits a different width
   depending on Arnold-tongue compression
   (\S\,planned).  This is the load-bearing quantitative gap.
4. **Convert lattice-unit $M_\text{min}(d)$ to physical units.**
   At a chosen lattice spacing $a$ (constrained by Paper~I §P7 to
   $a \le 10^{-19}$\,m), give the prediction for $M_\text{min}(d)$
   in physical mass and length units, for direct comparison to
   compact-object observations.

## Atomic ISCO (outline only)

The largest $d$ at which gradient ionization always succeeds for
$n=1$ -- i.e.\ the smallest distance from a fixed $M$ inside which
no neutral atom can exist -- comes from rearranging the threshold:

$$r_\text{atomic\_ISCO}(M) \;=\; \left( \frac{M \cdot R_1}{\Delta\omega_\text{tongue}} \right)^{1/3}.$$

This is the structural analogue of the geodesic ISCO at $r = 6
GM/c^2$.  Open question: does this radius coincide with the photon
sphere ($r = 3 GM/c^2$)?  The two have different scalings ($r
\propto M^{1/3}$ vs $r \propto M$), so they cannot agree at all
$M$; they may cross at one value, which would be the natural
identification of the framework's lattice scale $a$.  This is the
sharpest open follow-up in Paper~III's scope.

## Open follow-up

- The relativistic-infall regime (catalogue entry~\#10, Case~3 of
  Paper~I's catalogue): for a radially infalling atom, time
  dilation slows the internal clock while the tidal gradient
  detunes the resonance.  These two effects are not independent --
  both are aspects of the metric -- and they may partially cancel,
  extending neutral-hydrogen survival closer to the compact object
  than the static $M_\text{min}(d)$ predicts.  This is the natural
  Paper~IV in the series.

## Pointers

- `notes/plasma_as_gravitational_ionization.md` -- inherited from
  Paper~I; the theoretical companion to the gradient-ionization
  argument that this note translates into $M_\text{min}(d)$ form.
- `src/experiments/exp_18_tidal_ionization.py` -- the numerical
  g-scan that confirms $T_\text{escape}(g)$ drops at
  $g_\text{crit}$.
- Paper~I catalogue entry~\#9
  (`external/dcl/notes/follow_on_implications.md`, section "Tidal
  Ionization Mass") -- the original framing this note expands.
