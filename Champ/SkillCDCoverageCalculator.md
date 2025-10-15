# League of Legends Skill CD Coverage Calculator

## Problem Description

This calculator helps determine the attack speed and critical chance requirements for achieving perfect skill coverage on champions that rely heavily on a specific ability with the following characteristics:

- **Skill Duration**: $D$ seconds
- **Skill Cooldown**: $T$ seconds (fixed)
- **Item Effect**: Critical strikes reduce remaining cooldown by $p$%
- **Goal**: Achieve seamless skill coverage (zero downtime)

For the specific case analyzed: $D = 8$, $T = 17$, $p = 15$
> ![NOTE]
> Referring to the current PC version of Kog'Maw's W skill with a fixed 17-second cooldown and 8-second duration, combined with wildrift Navori's effect that reduces remaining cooldown of non-ultimate skills by 15% on critical hits.

## Mathematical Model

### Key Variables
- $AS$ = Attack Speed (attacks per second)
- $CR$ = Critical Chance (0.0 to 1.0)
- $r$ = Effective crit rate per second = $AS \times CR$
- $\alpha$ = CD reduction rate = $\frac{p}{100} = 0.15$

### CD Reduction Mechanics

During the $D$-second skill duration, the cooldown is affected by:
1. **Natural cooldown**: -1 second per second
2. **Critical strikes**: Each crit reduces remaining CD by $p$%

The continuous approximation differential equation:

$$\frac{dCD}{dt} = -1 - \alpha \times r \times CD(t)$$

### Solution

The cooldown at time $t$ is given by:

$$CD(t) = \left(T + \frac{1}{\alpha r}\right)e^{-\alpha rt} - \frac{1}{\alpha r}$$

## Perfect Coverage Formula

For perfect coverage, we need $CD(D) \leq 0$, which leads to:

$$\left(T\alpha r + 1\right)e^{-\alpha rD} \leq 1$$

**For our specific values ($T=17$, $D=8$, $\alpha=0.15$):**

$$\boxed{AS \times CR \geq r^*}$$

where $r^* \approx 1.89$ is the solution to:

$$(2.55r + 1)e^{-1.2r} = 1$$

## Quick Reference Table

| Critical Chance | Minimum Attack Speed Required |
|-----------------|-------------------------------|
| 100%           | 1.89                          |
| 75%            | 2.52                          |
| 50%            | 3.78                          |
| 25%            | 7.56                          |

## Usage

Simply multiply your champion's attack speed by critical chance ratio. If the result is ≥ 1.89, you can achieve perfect skill coverage.

### Example
- Attack Speed: 2.5
- Critical Chance: 75% (0.75)
- Calculation: 2.5 × 0.75 = 1.89
- Result: Perfect coverage achieved (1.89 $$\geq$$ 1.89)

## General Formula

For different skill parameters, solve:

$$\left(T\alpha r + 1\right)e^{-\alpha rD} = 1$$

where:
- $T$ = skill cooldown
- $D$ = skill duration  
- $\alpha$ = CD reduction percentage (as decimal)
- $r$ = $AS \times CR$

## Notes

- This calculation assumes uniform distribution of critical strikes
- Real gameplay may vary due to RNG and timing variations
- Consider adding a small buffer above the threshold for practical reliability
