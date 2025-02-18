# Conda, conda installs and conda environments
Conda is a useful tool that lets you create isolated python environments and
install packages into them. Three modules on Bunya provide conda.

- miniforge
- anaconda3
- miniconda3

We recommend using the miniforge module to manage python environments.


## Using Conda Modules
Conda environment modules are loaded with the `module load` command.
[software](Bunya-User-Guide.md#software)

### Anaconda
Load the module and initialise the environment with the following commands:<br>
`module load anaconda3`<br>
`source $EBROOTANACONDA3/etc/profile.d/conda.sh`

### Miniconda
Load the module and initialise the environment with the following commands:<br>
`module load miniconda3`<br>
`source $EBROOTANACONDA3/etc/profile.d/conda.sh`

### Miniforge
Load the module and initialise the environment with the following commands:<br>
`module load miniforge`<br>
`mf-activate`<br><br>
Notes about Miniforge
- Miniforge includes the `mamba` command in addition to `conda`. The mamba
  command can be used instead of conda and has the same syntax. Think of mamba
  as a faster version of conda—the two commands are interchangeable.
- Miniforge only provides open source packages whereas Anaconda can provide
  packages under the Anaconda Inc. license. For this reason we recommend using
  the Miniforge module. If we create a python environment using mamba we still
  call it a 'conda' environment, mamba is 100% compatible with a conda. Also,
  the configuration methods discussed here (e.g., the conda.rc file) also apply
  to mamba.

## Base conda environment
If you want to activate the base conda environment you can do<br>
`[username@bunya3 ~]$ conda activate`<br>
`(base) [username@bunya3 ~]$`<br> 

Exit the base conda environment with do<br>
`(base) [username@bunya3 ~]$ conda deactivate`<br>
`[username@bunya3 ~]$`<br>

You can run python and access the packages in the base environment, but you can
not install anything into it. To install packages create and activate your own
environment.

## Creating a new conda environment

Create a conda environment called `myenv` in the default location<br>
`conda create --name myenv`<br> A new environment will be created in the default
location (normally `/home/UserName/.conda`) with latest version of python
available for Bunya.


Please note: By default environments are installed into the `envs` directory in
your conda directory which is `/home/YourUsername/.conda`. If you need to
specify a particular location for an environment please
[here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#specifying-location).

2. When conda asks you to proceed type `y`

3. To create an environment with a specific python version, for example python 3.9:<br>
`conda create --name myenv python=3.9`

4. To create an environment with a specific package, for example scipy:<br>
`conda create --name myenv scipy`<br>
or<br>
`conda create --name myenv`<br>
`conda install --name myenv scipy`<br>

5.  To create an environment with a specific version of Python and multiple packages:<br>
`conda create --name myenv python=3.9 scipy=0.17.3 astroid babel`

Tip: Install all the programs that you want in this environment at the same
time. Installing 1 program at a time can lead to dependency conflicts.

## Settings the location of conda environments and package caches

On Bunya your home directory `/home/username` has 50GB of space and 1 million
files for environments and their packages. However, in case this is not enough
space in home you can place environments in `/scratch/user/username` where there
is more space available. The default location for environments can be set in the
[Conda configuration file](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html),
`.condarc` file. See specifically instructions on envs location
[here](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#specify-environment-directories-envs-dirs).

In your home directory open the `.condarc` file. You can use `nano` for this or `vi`, what ever you are comfortable with.

The insert these lines:

```
envs_dirs:
  - /scratch/rest-of-the-path-of-location/envs
```

Examples of environment locations can be

`/scratch/user/username/rest-of-path/envs`

or

`/scratch/project/project-name/rest-of-path/envs`

This will allow you to install environments and find them by name. You will not
need the full path to activate the environment.

## Activating a conda environment

`conda activate myenv`

## Deactivating a conda environment

`conda deactivate myenv`

## Advice for pip installs

If you need to install a mix of conda and pip packages, install the conda
packages first.

If you want to install only pip packages it is **highly recommended** that you
create a conda environment and install the pip packages into it. To do so create
the conda environment, activate it and run `conda install pip`. After conda
installs pip use pip to install your packages `pip install some-library`. **It
is important to install pip using conda first**

## Advice on installing your own conda

Avoid running the conda initialisation which writes to your `.bashrc` file. This
changes your shell permanently and can cause problems. If your prompt has a
`(base)` in it when you log in then your `.bashrc` file has already been
changed. You can reverse this by cleaning up your `.bashrc` file and sourcing
the `conda.sh` file from your installation. You can then use<br>
`conda activate`<br> to switch on the conda base environment and<br>
`conda deactivate` <br> to switch it off again. This is keeping the shell clean
and conda base and other conda environments can so be loaded for jobs only.

Users can clean their `.bashrc` file by opening it and removing everything
between and including these two lines<br> `# >>> conda initialize >>>`<br>
`# <<< conda initialize <<<`<br>

Users can also clean their `.bashrc` file by using `conda init` again with <br>
`conda init --reverse`<br>


## Advice on conda and onBunya usage

If you have the conda initialisation in your `.bashrc` file then you cannot use
Open OnDemand. To use the virtual desktop in Open OnDemand you need to have
clean `.bashrc` file. The easiest was to clean it is to run <br>
`conda init --reverse`<br>

For further information on conda environments please go
[here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#).

