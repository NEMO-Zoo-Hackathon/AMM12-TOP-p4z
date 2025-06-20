# 🌊 NEMO AMM12 + TOP + PISCES (p2z/p4z) Configuration

This repository documents the setup, configuration, and testing of the **NEMO ocean model** using the **AMM12 regional configuration**, extended with the **TOP biogeochemical model** and **PISCES (both p2z and p4z)**. The goal is to explore biological tracers and sediment interactions on the North-West European Shelf.

---

## Repository Structure
## Step 0: Download all_all_nemo.sh 
https://forge.nemo-ocean.eu/nemo/nst/-/wikis/uploads/2780c5b794ebd76a3c6271b53c53ac89/install_all_nemo.sh 
Compile:
Chmod u+x install_all_nemo.sh and then 
./all_all_nemo.sh



## Step 1: Base Configuration – AMM12 (No TOP)
Compile:

./makenemo -n AMM12_Hack -r AMM12 -m auto -j 4

Prepare input files:

cd $HOME/ALL_NEMO_default/nemo_5.0/sette/

./sette_fetch_inputs.sh

tar -xvzf AMM12_v5.0.0.tar.gz

mv AMM12_v5.0.0/* ../cfgs/AMM12_Hack/EXP00/

Run the model: example: ./nemo


## Step 2: Add Biogeochemistry with PISCES (p2z)

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

## Step 3: Enable Sediment Model (PISCES p4z)

Compile:

./makenemo -n AMM12_HackTop4z -r AMM12 -m auto -j 4

Activate in namelist_pisces_cfg:

ln_p4z = .true.

Run without the sediment first

Copy and configure:

Sediment namelists from SHARED/

ln_sed_2way = .true in namelist_sediment_cfg and namelist_pisces_cfg

Ensure sediment outputs in XML definitions



## 🤝 Acknowledgements

Thanks to mentors and contributors (Julien and Renaud) involved in debugging and testing this setup.

