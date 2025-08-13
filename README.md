SASFOX: Small-Angle Scattering Fitting and Output eXtractor

Version 1.0 — 20250813

Author: Dr. Alessandro Mariani

Description
SASFOX.py is a Python script for automated processing, fitting, and analysis of SAXS/SANS data. It can handle one or multiple datasets, apply background subtraction, normalize intensities, fit structural models, and extract key scattering parameters. The workflow is driven entirely by a user-editable config.ini file.

The code is designed for batch mode processing, enabling the rapid and reproducible analysis of multiple scattering curves with minimal manual intervention.

Main Features
	•	Reads one or more SAXS/SANS data files in plain text format.
	•	Supports background subtraction from a separate file or from the data itself.
	•	Unit handling for q values (Å⁻¹ or nm⁻¹).
	•	Optional intensity normalization with user-defined scaling factor.
	•	Automatic cropping of data to user-defined qmin and qmax.
	•	Fits data using various models (Guinier, Porod, power-law, etc.) depending on configuration.
	•	Extracts characteristic lengths (e.g., correlation length, radius of gyration).
	•	Outputs processed data and fitted parameters in CSV format.
	•	Saves publication-quality plots with fitted curves and residuals.
	•	Logs all processing steps and potential warnings for traceability.

Version History

v1.0 (2025-08-13)
	•	Initial stable release with config-driven execution.
	•	Added batch mode for multiple input files.
	•	Unified plotting style and export routines.
	•	Standardized logging format.

Usage
	1.	Requirements
Python 3.x with:
pip install numpy matplotlib scipy configparser argparse
	2.	Prepare the Input File
All parameters are set in config.ini.

Main sections in config.ini:
	•	[INPUT]
	•	input_files — space-separated list of SAXS/SANS data files to process.
	•	header_lines — number of header lines to skip in each data file.
	•	units — A for Å⁻¹, nm for nm⁻¹.
	•	normalization_factor — scalar multiplier for intensities.
	•	qmin, qmax — cropping limits in q.
	•	background_file — optional background scattering file for subtraction.
	•	[FITTING]
	•	model — fitting model to apply (e.g., guinier, porod, powerlaw).
	•	initial_parameters — starting guesses for fitting parameters.
	•	[PLOTTING]
	•	plot_title — custom title for plots.
	•	save_plots — yes or no.
	•	output_directory — where plots and processed data will be saved.

	3.	Running the Program
From the terminal:
python SASFOX.py path/to/config.ini

Outputs
	•	Data files: processed intensity data, fitted parameters, residuals.
	•	Plots:
	•	Experimental data with fit overlay.
	•	Residuals (data − fit).
	•	Log file: records all steps and any warnings or errors.

Notes
	•	Ensure all file paths in config.ini are valid and accessible.
	•	Units of q and intensities must be consistent.
	•	Fitting convergence depends strongly on initial parameter guesses — adjust them if fits are poor.
