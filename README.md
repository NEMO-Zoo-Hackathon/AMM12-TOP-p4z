# NEMO AMM12 + TOP + PISCES (p2z/p4z) Configuration
This repository documents the setup, configuration, and troubleshooting progress for running the NEMO ocean model with the AMM12 configuration, extended to include the biogeochemical modules TOP with PISCES (p2z and p4z) and sediment interaction.



# 🌊 NEMO AMM12 + TOP + PISCES (p2z/p4z) Configuration

This repository documents the setup, configuration, and testing of the **NEMO ocean model** using the **AMM12 regional configuration**, extended with the **TOP biogeochemical model** and **PISCES (both p2z and p4z)**. The goal is to explore biological tracers and sediment interactions on the North-West European Shelf.

---

## Repository Structure

.
├── EXP00/                   # Run directory for experiments
├── SETTE/                  # For test input fetch & setup
├── namelist_cfg            # Main namelist
├── namelist_top_cfg        # TOP module config
├── context_nemo.xml        # XIOS context file
├── file_def_nemo-*.xml     # Output file definitions
├── field_def_nemo-*.xml    # Field definitions for diagnostics
└── README.md               # This file


Step 1: Base Configuration – AMM12 (No TOP)
Compile:

./makenemo -n AMM12_Hack -r AMM12 -m auto -j 4
Prepare input files:

./sette_fetch_inputs.sh
tar -xvzf sette/AMM12_v5.0.0.tar.gz
mv AMM12_v5.0.0/* ../cfgs/AMM12_Hack/EXP00/
Run the model:

./nemo

Output includes SST, SSS → successfully validated

Step 2: Add Biogeochemistry with PISCES (p2z)

Compile:
./makenemo -n AMM12_HackTop -r AMM12 -m auto -j 4
Add TOP to work_cfgs.txt

Copy base EXP00:

cp -r AMM12_Hack/EXP00/* AMM12_HackTop/EXP00/
Add XML config files from ORCA2_OFF_PISCES:

field_def_nemo-pisces.xml

file_def_nemo-pisces.xml

Edit context_nemo.xml to include TOP definitions

Run with and without open boundaries: ln_bdy = .true. / .false.

Step 3: Enable Sediment Model (PISCES p4z)

Compile:
./makenemo -n AMM12_HackTop4z -r AMM12 -m auto -j 4
Activate in namelist_pisces_cfg:

ln_p4z = .true.
Copy and configure:

Sediment namelists from SHARED/

ln_sed = .true. in namelist_cfg

Ensure sediment outputs in XML definitions



Output Examples
Surface Chlorophyll & NO3 plots

Bottom Oxygen animation

Output available under AMM12_1d_*_ptrc_T.nc

📎 References
NEMO Official Site

PISCES documentation

AMM12 setup derived from ORCA2_OFF_PISCES and SETTE

🤝 Acknowledgements
Thanks to mentors and contributors involved in debugging and testing this setup, especially during the Hackathon phase.

📧 Contact
For questions or feedback, open an issue or contact [atall@eoas.ubc.ca].
