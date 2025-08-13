SASFOX: Small-Angle Scattering Fitting and Output eXtractor

Author: Dr. Alessandro Mariani

Description
SASFOX.py processes SAXS/SANS curves in batch, applies optional background subtraction, lets you define the analysis window (qmin–qmax) interactively or by flags, and extracts correlation lengths by (i) model-free integrals, (ii) Ornstein–Zernike (OZ) fitting, and (iii) Debye–Bueche (DB) fitting. It also provides an interactive, user-selected range to estimate a power-law slope and a Guinier-style radius of gyration (Rg), with residuals exported for the selected region. All results and plots are written to a timestamped output folder.

Key features
• Batch processing of one or more 2-column data files (q, I)
• Optional background subtraction from a separate (q_bg, I_bg) file (nearest-q match with 5% tolerance)
• Units for q: A (Å⁻¹) or nm (nm⁻¹); internally q is converted to nm⁻¹ (Å⁻¹ → ×10)
• Intensity normalization via a scalar factor (multiplies I)
• q-range set by flags or by clicking two points on a plot (background curve if provided, otherwise the first input curve)
• Model-free quantities over [qmin, qmax]:
– Invariant  Q = ∫ I(q) q² dq
– Integrated intensity  J = ∫ I(q) q dq
– “No-model” correlation length  ξ_int = J / Q
• Model fits over [qmin, qmax]:
– Ornstein–Zernike:  I(q) = I₀ / [1 + (q ξ)²]
– Debye–Bueche:      I(q) = I₀ / [1 + (q ξ)²]²
Both return parameter standard errors and fit metrics (n, k, dof, RSS, TSS, R², R²_adj, RMSE, χ²_red, AIC, BIC).
• Interactive power-law/Guinier region: pick a range; the code estimates a power-law slope and an Rg, and writes residuals for that region (see “Outputs”).
• Per-file console summary plus a tab-delimited master table (results.txt).

Command-line usage
python SASFOX.py -i  [-hl ] [-u {A,nm}] [-f ] [-qmin ] [-qmax ] [-bg <bg_file>]

Arguments
-i, –input           Space-separated list of input data files (required).
-hl, –header_lines   Number of header lines to skip (default: 0).
-u, –units           q units: A (Å⁻¹) or nm (nm⁻¹). Default: A.
-f, –factor          Intensity normalization factor (default: 1.0).
-qmin, -qmax          q-window for analysis. If either is omitted, SASFOX asks you to click two points to set them.
-bg, –background     Optional background file (same format as inputs). If given, background is subtracted after mapping to the closest q (5% tolerance on |Δq|/q).

Input format
• Plain text, whitespace-delimited, two numeric columns:
col 0 = q, col 1 = I(q).
• Use -hl if your files contain headers.
• If using a background, its q-grid should closely match data; otherwise subtraction aborts when |q_data – q_bg| / q_data > 5%.

Outputs (created in a new folder named YYYYMMDD_SAXSanalysis_N)
	1.	Per-file plots
• _OZ.png — data and OZ fit over [qmin, qmax]
• _DB.png — data and DB fit over [qmin, qmax]
• _OZ_residuals.png — OZ residuals (log x; linear y fixed to ±1×10⁻³)
	2.	Residual tables (tab-separated; columns: q, I_obs, I_fit, residual, std_residual[NaN])
• _OZ_residuals.tsv
• _DB_residuals.tsv
• _Guinier_residuals.tsv  (note: despite the name, this records power-law residuals over your user-selected range)
	3.	Master results table
• results.txt (tab-separated; one row per input). Columns include:
– Invariant Q, Integrated intensity J, “No-model” ξ_int = J/Q
– OZ fit: ξ (reported as “OZ Correlation Length”), I₀, standard errors, and metrics (n, k, dof, RSS, TSS, R², R²_adj, RMSE, χ²_red, AIC, BIC)
– DB fit: ξ (reported as “DB Correlation Length”), I₀, standard errors, and the same metrics
– Interactive range estimates: power-law slope and Rg
– If the interactive fit computed metrics, their n, k, dof, RSS, TSS, R², R²_adj, RMSE, χ²_red, AIC, BIC are appended
	4.	Run directory
• analysis folder with all files above; naming auto-increments to avoid overwriting.

Typical workflows
• Fully interactive q-window (no background):
python SASFOX.py -i sample1.txt sample2.txt -hl 1 -u A -f 1.0
(Click two points to set qmin and qmax; repeat range selection when prompted for the power-law/Guinier step.)
• With background and explicit q-window:
python SASFOX.py -i data/*.dat -u nm -bg background.dat -qmin 0.1 -qmax 4.0

Notes & assumptions
• Units: Internally q is treated in nm⁻¹. If you pass Å⁻¹ (-u A), SASFOX multiplies q by 10. Correlation lengths (ξ, Rg) are therefore in nm.
• Background subtraction: nearest-neighbor mapping in q with |Δq|/q ≤ 0.05; outside this tolerance, the run stops with an error.
• Metrics: χ²_red is reported but not modeled with experimental σ; in this build it remains NaN unless you extend the code.
• “Guinier_residuals.tsv” file name is historical; it actually logs residuals for the power-law fit over your selected range.
• Input files are read with pandas.read_csv(…, delim_whitespace=True, usecols=[0,1], header=None). Ensure the first two columns are q and I.
• Plots use log–log axes for data/fit; OZ residuals PNG is produced, DB residuals PNG is not (TSV only).
• All integrals and fits are restricted to [qmin, qmax] after optional background subtraction.

Requirements
Python 3.x with: numpy, pandas, scipy, matplotlib.
Install example:
pip install numpy pandas scipy matplotlib

Contact
Issues or feature requests: please include your command line, a small input example, and the produced results.txt.
alessandro1.mariani@polimi.it
