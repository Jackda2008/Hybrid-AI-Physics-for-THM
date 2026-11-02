Hybrid AI–Physics for Coupled THM Processes

Data and Python code accompanying:

Hu G. (2026). Accelerating coupled transport modeling: Hybrid AI-Physics approach for thermo-hydro-mechanical coupled processes. AI Thermal Fluids, 6, 100042. https://doi.org/10.1016/j.aitf.2026.100042

The notebook compares three surrogate models for coupled thermo-hydro-mechanical (THM) processes around a heated, pressurised borehole in a low-permeability host rock, as relevant to deep geological disposal of radioactive waste:

Model	Idea
Pure NN	Purely data-driven feedforward network (baseline)
PINN	Physics-informed neural network: data loss + residuals of simplified hydraulic, thermal and mechanical equations
Hybrid AI–Physics	An analytical physics model (radial diffusion solutions for pressure and temperature, thermo-poroelastic displacement) gives a first prediction; a neural network learns the correction. The final output is a weighted combination of the two

All models are trained and evaluated against reference simulations made with OpenGeoSys (OGS).

Repository contents
Data and Python Code of Hybrid AI-Physics for THM.ipynb   # data, models, training, evaluation and figures
README.md
LICENSE

The OGS reference data are embedded directly in the notebook, so no external data download is needed.

Data
Case: borehole heating and pressurisation test case (AnSichT parameters). Borehole radius 0.25 m, far field 10 m; borehole conditions 6 MPa and 100 °C; initial state 4 MPa and 25 °C.
Outputs: pore pressure P (Pa), temperature T (°C) and radial displacement u (m).
Sampling: OGS time series at four distances from the borehole (x = 0.5, 1.0, 2.5 and 5.0 m) over one year (0–365 days). After interpolation, this gives 320 samples with inputs (x, t).
Split: 70 % training / 30 % testing (random_state = 42). Further cells evaluate test ratios from 0.3 to 0.7.
Notebook structure
Cell	Content
1	Configuration, embedded OGS data, preprocessing (standardisation, train/test split)
2	Model definitions: PureNN, PINN (autograd PDE residuals), PhysicsModel + HybridModel
3	Loss functions and training loops
4	Training of all models; metrics (R², MAE, RMSE, MAPE) per output, compared with the physics-only model
5	Figures: training curves, predictions vs. OGS, temporal profiles, 2D fields, error analysis
6–9	Figures: performance metrics, sensitivity to the test ratio, and computational efficiency compared with OGS

Default settings: 5 hidden layers × 128 neurons; Adam (lr = 1e-3); 5000 epochs; batch size 32. Random seeds are fixed (42) for reproducibility.

Requirements

Tested with Python 3.12.

bash
pip install numpy pandas scipy scikit-learn matplotlib torch

A GPU is optional. On a CPU, training takes about 20 minutes each for the pure NN and the hybrid model, and about 2 hours for the PINN.

Usage
bash
git clone https://github.com/Jackda2008/Hybrid-AI-Physics-for-THM.git
cd Hybrid-AI-Physics-for-THM
jupyter notebook "Data and Python Code of Hybrid AI-Physics for THM.ipynb"

Run the cells in order. The figures are written as PNG files to the working directory. They use Times New Roman; if that font is not installed, matplotlib falls back to a default font and prints a warning.

Note: figure cells 6–9 plot summary values from earlier runs, which are written directly into the code; cells 1–5 recompute the results from scratch.

Citation

If you use this code or data, please cite:

bibtex
@article{Hu2026HybridTHM,
  author  = {Hu, Guang},
  title   = {Accelerating coupled transport modeling: Hybrid AI-Physics approach for thermo-hydro-mechanical coupled processes},
  journal = {AI Thermal Fluids},
  volume  = {6},
  pages   = {100042},
  year    = {2026},
  doi     = {10.1016/j.aitf.2026.100042}
}
License

Released under the MIT License; see LICENSE.

Contact

Guang Hu, Energy Technology Group, Department of Mechanical Engineering, Eindhoven University of Technology (TU/e), the Netherlands.
