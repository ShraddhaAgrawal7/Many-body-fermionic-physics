The simulation computes the spin-echo Ramsey contrast of a 1D chain of nuclear-spin-polarized ⁸⁷Sr atoms in a harmonic gradient. 
The aim is to find when the initial contrast collapse scales as t_x² (superexchange) and when it scales as t_x (hole hopping). 
It evolves the full many-body state exactly, with no fitting parameters: t_x, U, the gradient κ, the offset δ0 and the filling are inputs.
Default inputs: U = 800 Hz, t_x = 10–30 Hz, κ = 50 Hz/site, chain length L = 9 sites, readout over the central 5 sites (±1.1 μm).

Model

Each atom is a pseudo spin $\downarrow$ = g ($^1S_0$), and $\uparrow$ = e ($^3P_0$). The chain is a plain t-J model with a site-dependent potential. 

```math
\begin{aligned}
H &= -t \sum_{j,\sigma} \mathcal{P}
\left(c^\dagger_{j+1,\sigma} c_{j,\sigma} + \mathrm{h.c.}\right)
\mathcal{P} \\
&\quad + \sum_j V_j n_j
+ \sum_j J_j \left(
\mathbf{S}_j \cdot \mathbf{S}_{j+1} - \tfrac{1}{4} n_j n_{j+1}
\right).
\end{aligned}
```
The projector removes doubly occupied sites. The potential is harmonic plus a uniform tilt $\delta_0$. The offset bond grows linearly like so:

```math
V_j = \tfrac{\kappa}{2} x_j^2 + \delta_0 x_j, \qquad \Delta_j = V_{j+1} - V_j = \kappa\left(x_j + \tfrac{1}{2}\right) + \delta_0
```

We use the superexchange rate to be $J_j = \frac{2t^2}{U-\Delta_j} + \frac{2t^2}{U+\Delta_j} = \frac{4t^2 U}{U^2-\Delta_j^2}$

The clock laser's spin-orbit phase does not appear in H. The gauge transformation c_{j,↑} → e^{iφ x_j} c_{j,↑} moves it from the hopping into the pulses, with φ = k_c a = 1.165π. H is then real and SU(2)-symmetric, and each pulse acts on site j with laser phase φ x_j (see Pulse sequence below).


Basis and matrix construction

The tJ Hamiltonian conserves the atom number N and the number of e atoms m, so the matrix is built and diagonalized one (N, m) sector at a time. The pulses change m; they are applied as maps between sectors. It is easier to see the construction of the matrix with an example:

3 sites, 2 atoms, one in e: The sector N = 2, m = 1 on a 3-site chain has 3 × 2 = 6 states. Inputs: t = 20 Hz, U = 800 Hz, κ = 50 Hz/site, δ0 = 0. Then x = −1, 0, 1; V = 25, 0, 25 Hz; Δ = −25, +25 Hz on the two bonds; J = 4t²U/(U² − Δ²) = 2.002 Hz on both bonds.

We choose a basis and arrange it in a way that makes the Hamiltonian symmetric:

| State | Occupied sites x | Spins, left to right | V sum (Hz) |
| ---: | --- | :---: | ---: |
| 1 | −1, 0 | ↑↓ | 25 |
| 2 | −1, 0 | ↓↑ | 25 |
| 3 | −1, +1 | ↑↓ | 50 |
| 4 | −1, +1 | ↓↑ | 50 |
| 5 | 0, +1 | ↑↓ | 25 |
| 6 | 0, +1 | ↓↑ | 25 |

The matrix/Hamiltonian becomes:

```math
H = \begin{pmatrix}
23.999 & 1.001 & -20 & 0 & 0 & 0 \\
1.001 & 23.999 & 0 & -20 & 0 & 0 \\
-20 & 0 & 50 & 0 & -20 & 0 \\
0 & -20 & 0 & 50 & 0 & -20 \\
0 & 0 & -20 & 0 & 23.999 & 1.001 \\
0 & 0 & 0 & -20 & 1.001 & 23.999
\end{pmatrix}
```

The diagonals for state 1, 2, 5, 6 are V+ (-J/2) so thats 23.999 Hz. Diagonals of states 3, 4 are $\Delta = 50$. Exchange is between 1 and 2, and between 5 and 6 is J/2. Tunneling term of −20 Hz between 1 and 3: the atom on x = 0 hops to x = +1, with the spin order kept (↑↓ stays ↑↓). Between 3 and 5: the atom on x = −1 hops to x = 0.

Initial states and loading:
Each run starts from one occupation pattern c: atoms sit on definite sites (one Wannier state each) and all are in g. The first pulse (tip angle $\theta = 0.7 \pi$) turns this into a product state, with the laser phase φx of each atom's site:

```math
|\psi_c(0)\rangle = \bigotimes_{k=1}^{N}\left[\cos\tfrac{\theta_0}{2}\,|{\downarrow}\rangle_{p_k} - i\sin\tfrac{\theta_0}{2}\,e^{i\varphi x_{p_k}}\,|{\uparrow}\rangle_{p_k}\right]
```

The z-average models imaging along z: each ROI column contains many chains, each with its own filling n(z) = min(1, n_peak e^{−z²/2σ_z²}). The camera sums their signals, so the column's weights are Σ_z w_c(n(z)). The reported filling is the atom-weighted column mean, Σn²/Σn; it does not depend on σ_z.

Assumption to keep in mind: a pattern with atoms on definite sites is not an eigenstate where t_x > Δ_j. If loading is adiabatic, atoms on those central bonds start delocalized, and the early hole-hopping drop would differ.


Pulse sequence, time evolution and readout:

The sequence is 0.7π tip → T → π echo → T → readout. T is one arm, so the total dark time is 2T. Pulses are instantaneous rotations on each atom, with a phase set by the atom's site:

```math
R_j(\theta) = \exp\left[-\tfrac{i\theta}{2}\left(e^{i\varphi x_j}\sigma^+_j + e^{-i\varphi x_j}\sigma^-_j\right)\right]
```

The echo π pulse flips every spin. A state (c, s) in sector m maps to (c, s̄) in sector N − m with phase Π_k (−i) e^{±iφx_k}: + for ↓→↑, − for ↑→↓. 

Time evolution is given by $|\psi_c(2T)\rangle = U(T)\,R_\pi\,U(T)\,|\psi_c(0)\rangle$. 

The readout $\pi/2$ pulse is scanned in phase and the phase and contrast are:
O = \sum_{j \in \mathrm{ROI}} e^{-i\varphi x_j}\,\sigma^-_j, \qquad C(T) = \frac{2\left|\sum_c w_c \langle O\rangle_c\right|}{\sum_c w_c \langle n_{\mathrm{ROI}}\rangle_c}

The complex signals of all initial states are summed.
