# Making [ENCODE ChIP-seq pipeline](https://github.com/ENCODE-DCC/chip-seq-pipeline2) work on [MSI](https://www.msi.umn.edu)

## Installation
It's worth doing this interactively, since it will take a while. This repo is identical to the main repo except for some changes to the install script to make a local conda install with the `--prefix` command. I recommend installing this repo to your groups `shared` directory, that way it's easily usable across your lab. Furthermore, this install will add over 100K files to your groupquota, so you don't want it replicated a bunch!

```
# Request an interactive session

srun --nodes=1 --ntasks-per-node=10 --cpus-per-task=1 --time=03:00:00 --mem=10GB --account=<group_name> --partition=interactive --pty bash

# Load Python3 module with mamba
module load python3/3.8.3_anaconda2020.07_mamba

cd /home/<group_name>/shared 
git clone https://github.com/acherman/chip-seq-pipeline2.git
cd chip-seq-pipeline2/

# This might take a while!
bash scripts/install_conda_env.sh mamba
```


## Activation and de-activation
```
module load python3/3.8.3_anaconda2020.07_mamba

# Use this if you don't have your shell configured or haven't run conda (either ever, or in your current session)
source activate /home/group/me/software/chip-seq-pipeline2/encode-chip-seq-pipeline

# Otherwise, use this
conda activate /home/group/me/software/chip-seq-pipeline2/encode-chip-seq-pipeline

# De-activation
conda deactivate
```

## Dealing with `bowtie2` error
There seems to be a problem with `tbb` library access in bowtie2. While not exactly the same issue, I believe the solution can be [found here](https://forum.biobakery.org/t/workflow-conda-install-bowtie-2-issue-bowtie2-align-s-error-while-loading-shared-libraries-libtbb-so-2/1831). Essentially, we need to install a particular version of `tbb`.

```
module load python3/3.8.3_anaconda2020.07_mamba

source activate /home/group/me/software/chip-seq-pipeline2/encode-chip-seq-pipeline

conda install tbb=2020.2
```
This should hopefully do the trick!

## TBD 
I do not know how this software is actually run! Once this is figured out, I'll add a job script example. If it's easier to run interactively, it's easiest to modify the `srun` command above for more memory, CPUs, and walltime.

