The simulation computes the spin-echo Ramsey contrast of a 1D chain of nuclear-spin-polarized ⁸⁷Sr atoms in a harmonic gradient. 
The aim is to find when the initial contrast collapse scales as t_x² (superexchange) and when it scales as t_x (hole hopping). 
It evolves the full many-body state exactly, with no fitting parameters: t_x, U, the gradient κ, the offset δ0 and the filling are inputs.
Default inputs: U = 800 Hz, t_x = 10–30 Hz, κ = 50 Hz/site, chain length L = 9 sites, readout over the central 5 sites (±1.1 μm).

Model

Each atom is a pseudo spin $\downarrow$ = g ($^1S_0$), and $\uparrow$ = e ($^3P_0$). The chain is a plain t-J model with a site-dependent potential. 


$H = -t \sum_{j,\sigma} \mathcal{P}\left(c^\dagger_{j+1,\sigma} c_{j,\sigma} + \mathrm{h.c.}\right)\mathcal{P} + \sum_j V_j n_j + \sum_j J_j \left(\mathbf{S}_j\cdot\mathbf{S}_{j+1} - \tfrac{1}{4} n_j n_{j+1}\right)$
