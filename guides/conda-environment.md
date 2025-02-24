# Conda
Conda is a Python environment and package manager. It supports isolated
environments and enables you to manage dependencies for your Python projects. It
provides a large set of precompiled binaries for applications such as
Tensorflow, PyTorch, NumPy, Pandas, and many more.

## Conda Channels  

Conda channels are repositories that host precompiled packages.

### Defaults Channel
By default, Conda pulls packages from the `defaults` channel, maintained by Anaconda, Inc. The use of the `defaults` channel is subject to Anaconda Inc's licensing terms [🢅](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/channels.html#what-is-a-channel).

> [!NOTE]
> The University of Queensland is licensed under Anaconda Inc.’s commercial
> terms to use packages from the `defaults` channel.

Packages available in `defaults` are curated by Anaconda Inc. who prioritise stability and compatibility. Some applications may also include commercial or proprietary optimisations.

### Conda Forge Channel
`conda-forge` is a community-driven open-source channel. It offers a broader
selection of packages, which are generally more up to date that those in
`defaults`. There are no licensing requirements on using the `conda-forge`
channel itself, though individual packages retain their own licenses.<br><br>
For most use cases `conda-forge` is recommended due to its broader
package selection, frequency of updates, and lack of licensing restrictions.

> [!IMPORTANT]
> Licensing discussed here refers to use of the _channel_ and not the _packages_
> they contain. Individual packages have their own licensing terms. For example,
> packages available on both `defaults` and `conda-forge` are typically covered
> by open-source licenses. However, the `defaults` channel is more likely to
> include packages with proprietary components or licensing restrictions.

### Other Channels
Other channels are available such as:
- `bioconda` — bioinformatics and genomics software
- `nvidia` — GPU-accelerated libraries
- `pytorch` — official PyTorch packages
- `bioconda` — bioinformatics and genomics software
- `nvidia` — GPU-accelerated libraries
- `pytorch` — official PyTorch packages

These channels should be used as needed on a case-by-case basis. In most cases,
the required packages can be found on `conda-forge`, but certain specialized
packages may only be available in specific channels.

> [!NOTE]
> Installing packages, such as GPU-accelerated libraries, can usually be done
> using the `conda-forge` channel. A specialised chanel such as `nvidia`, should
> be used when there is a special need to do so.



To specify channels, use the `-c` flag during installation:  

```bash
conda install -c conda-forge numpy
```

Conda is a useful tool to create isolated python environments and
install packages. Three modules on Bunya provide conda.

- miniforge
- anaconda3
- miniconda3

We recommend using the miniforge module to manage python environments.


## Using Conda Modules
Conda environment modules are loaded with the `module load` command
[🢅](Bunya-User-Guide.md#software). After loading a module a further command is
required to initialise it. This is done as follows:

### Miniforge
Load the module and initialise the environment with the following commands:<br><br>
`module load miniforge`<br>
`mf-init`<br><br>
- Miniforge includes the `mamba` command in addition to `conda`. The mamba
  command can be used instead of conda and has the same syntax. Think of `mamba`
  as a faster version of `conda`—the two commands are interchangeable.<br>
- When `mamba` is used to create an environment, it is still referred to as a
  _conda_ environment. Also, the configuration methods discussed here (e.g., the
  conda.rc file) also apply to `mamba`. `mamba` is 100% compatible with `conda`.
- By default Miniforge provides open source packages from the conda-forge channel
  whereas, Anaconda can provide packages under the Anaconda Inc. license. For
  this reason we recommend using the Miniforge module.

### Anaconda
Load the module and initialise the environment with the following commands:<br><br>
`module load anaconda3`<br>
`source $EBROOTANACONDA3/etc/profile.d/conda.sh`<br><br>
- By default Anaconda provides packages from it's _default_ channel which may
  require paid licenses
  [🢅](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/channels.html#what-is-a-channel).
  For this reason we recommend using Miniforge if possible. If you require a
  package only available from Anaconda ensure that you have complied with any
  licence requierments.

### Miniconda
Load the module and initialise the environment with the following commands:<br><br>
`module load miniconda3`<br>
`source $EBROOTANACONDA3/etc/profile.d/conda.sh`<br><br>
- Using Miniconda module is practically the same as the Anaconda module for a
  Bunya user. Installing packages may download more files compared to the
  Anaconda module but you probably won't notice much difference.
- Miniconda has the same licence issues as Anaconda.

# Conda Environments
Conda environments should not be confused with the
[conda modules](#conda-modules). The modules contain software to manage
environments whereas, conda environments contain software and packages that run
your python applications. After loading and initialising a conda module (e.g.,
Miniconda) you can activate, deactivate, create and delete environments.

## Activating
To activate an environment stored in your default environment home, use:<br><br>
`conda activate my-env`<br><br> After running this command python has access to
packages and software installed in the my-env environment.<br> You can also
activate an environment from other locations by specifying the path to the
environment using:<br><br>`conda activate -p
/scratch/project/big-data-project/our-env`<br>

## Deactivating
Deactivate your current environment with:<br><br>`conda deactivate`<br><br>Environments must be
deactivated before they can be deleted.

## The Base Environment
If you want to simply run python without installing any packages you may do this
from the base environment. Activate the base conda environment with:<br><br>
`conda activate`<br><br> You can run python and access any packages in the base
environment, but you can't install anything into it. To install packages create
and activate your own environment.

## Creating an Environment
Create a conda environment called `myenv` in the default location<br><br>
`conda create myenv`<br><br> A new environment will be created in the default
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

