# Installing the CASPER toolflow for RFSoC hardware and testing the setup

Thanks to Mitch Burnett, Jack Hickish, Derek McKay and Amleset Kelati!

## Initial setup

It is assumed that you have:

- Ubuntu 18.04 OS environment
- MATLAB version R2019a is installed in /opt.
- Xilinx Vivado version 2020.2 is also installed in /opt

Add the following lines to the end of your `.bashrc`:

```
export XILINX_LOCAL_USER_DATA=no
export XILINXD_LICENSE_FILE=/opt/Xilinx/Xilinx.lic
export MLIB_DEVEL_PATH=$HOME/casper/mlib_devel
```
and run 
```
source ~/.bashrc
```

## Set up miniconda3 virtual environment and activate it

```
cd 
mkdir casper
cd casper
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
Review and accept the license.

Install Miniconda3 to `$HOME/casper/miniconda3`.

Run conda init:
```
Do you wish the installer to initialize Miniconda3
by running conda init? [yes|no]
[no] >>> yes

```
Close and re-open your current shell. 

Create casper environment using a definition file (thanks to Mitch Burnett for providing the original file!) that can be downloaded from here: [casper-env.yml](https://raw.githubusercontent.com/KajWiik/Casper_RFSoC/main/casper-env.yml).

```
cd casper
conda env create -f casper-env.yml
conda activate casper-env
```

## Install mlib_devel and Xilinx libraries for RFSoCs

In `casper-env` environment
```
rm -rf mlib_devel # if needed
rm -rf test_project # if needed
cd ~/casper
git clone https://gitlab.ras.byu.edu/alpaca/casper/mlib_devel.git
cd mlib_devel
git checkout -b rfsoc origin/rfsocs/devel
pip install -r requirements.txt

mkdir xilinx
cd xilinx
git clone https://github.com/xilinx/device-tree-xlnx.git
cd ~/casper/mlib_devel
```
Edit your `startsg.local` to:

```
XILINX_PATH=/opt/Xilinx/Vivado/2020.2
MATLAB_PATH=/opt/MATLAB/R2019a
PLATFORM=lin64
JASPER_BACKEND=vitis
export XLNX_DT_REPO_PATH=/$HOME/casper/xilinx/device-tree-xlnx
```
## Test the toolflow

Download the RFSoC tutorials:

```
cd ~/casper
git clone https://github.com/casper-astro/tutorials_devel.git

```

Start the toolflow by running
```
./startsg
```

In matlab prompt type `simulink`.


In Simulink window click `Open...` and select `~/casper/tutorials_devel/rfsoc/tut_platform/zcu111_tut_platform.slx`.

In the Simulink design window, type `Ctrl-D`. then in Matlab window type `jasper`.

After a while the log stream in the Matlab window should end to messages like this:

```
hsi::generate_target: Time (s): cpu = 00:00:29 ; elapsed = 00:00:32 . Memory (MB): peak = 1800.582 ; gain = 0.000 ; free physical = 314824 ; free virtual = 380405
Assembling jasper dt node
[32m[INFO    ]: Assembling jasper dt node[0m
Created /home/donald/casper/tutorials_devel/rfsoc/tut_platform/zcu111_tut_platform/outputs/zcu111_tut_platform_2022-02-28_1150.dtbo
****************************************
*  Backend complete!                   *
****************************************
```

