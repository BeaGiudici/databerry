#software 

Miniconda is a free, miniature installation of Anaconda Distribution that includes only conda, Python, the packages they both depend on, and a small number of other useful packages.

## Installation
For a Raspberry Pi, the `x86_64` version is not recommended. However, the `aarch64` runs smoothly.
Download the bash script
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
```
and run it with
```
bash Miniconda3-latest-Linux-aarch64.sh
```
Follow the prompts, and once it is installed it will create the directory `miniconda3` in your home folder (by default, or in the directory you choose during the installation).

## Commands
- Create a new environment: `conda create --name <env-name>`
- Activate an environment: `conda activate <env-name>
- Deactivate the active environment: `conda deactivate`
- Install a package: `conda install <package-name>` (also works with `pip`)
- Remove a package: `conda remove <package-name>`
- Create an environment from environment file: `conda env create -f environment.yml`
- 