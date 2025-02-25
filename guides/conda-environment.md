# Conda
Conda is a Python environment and package manager. It supports isolated
environments and allows you to manage dependencies for different Python projects
without interfering with system-wide packages or each other. It also provides a
large set of precompiled binaries for applications such as Tensorflow, PyTorch,
NumPy, Pandas, and many more.

## Conda Channels
Conda channels are repositories that host precompiled packages.

### Defaults Channel
By default, Conda pulls packages from the `defaults` channel, maintained by
Anaconda, Inc. The use of the `defaults` channel is subject to Anaconda Inc's
licensing terms
[🢅](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/channels.html#what-is-a-channel).

Packages available in `defaults` are curated by Anaconda Inc. who prioritise
stability and compatibility. Some applications may also include commercial or
proprietary optimisations.

> [!NOTE]
> The University of Queensland is licensed under Anaconda Inc.’s commercial
> terms to use the `defaults` channel.

### Conda Forge Channel
`conda-forge` is a community-driven open-source channel. It offers a broader
selection of packages, which are generally more up to date that those in
`defaults`. There are no licensing requirements on using the `conda-forge`
channel itself, though individual packages retain their own licenses.

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
- `bioconda` – bioinformatics and genomics software
- `nvidia` – GPU-accelerated libraries
- `pytorch` – official PyTorch packages

These channels should be used as needed on a case-by-case basis. In most cases,
the required packages can be found on `conda-forge`, but certain specialized
packages may only be available in specific channels.

> [!NOTE]
> Installing packages, such as GPU-accelerated libraries, can usually be done
> using the `conda-forge` channel. A specialised channel such as `nvidia`, should
> be used when there is a special need to do so.

## Conda Modules
Several Conda modules are available on Bunya for managing Python environments.

- `miniforge`
- `anaconda3`
- `miniconda3`

We recommend using the `miniforge` module for creating and managing Python
environments. If you are using an existing Python environment, it is best to
load the same module used to create it.

Note that multiple versions of these modules exist—the latest version of
`miniforge` is `miniforge/24.11.3-0`. Conda modules are loaded using the
`module load` command. Information on the module system and how to specify
versions can be found in the [Bunya User Guide](Bunya-User-Guide.md#software).

### **Loading and Initialising Conda Modules**
If no module version is specified, the latest available version of is loaded by
default. After loading a module, an additional step is required to initialise
it.

---
### **Miniforge**  
Load the module and initialize the environment with the following commands:  

```bash
module load miniforge
mf-init
```
### Miniforge Features
- Miniforge includes the `mamba` command in addition to `conda`.
- `mamba` is a drop-in replacement for `conda`, offering faster dependency
  resolution. The two commands are interchangeable.
- Environments created with `mamba` are still referred to as **Conda**
  environments and follow the same configuration methods (e.g., conda.rc).
- By default, Miniforge uses the `conda-forge` channel for package management.

---
### **Anaconda**
Load the module and initialize the environment with the following commands:

```bash
module load anaconda3
source $EBROOTANACONDA3/etc/profile.d/conda.sh
```
### Anaconda Features
- By default, Anaconda provides packages from the `defaults` channel.

---
### **Miniconda**
Load the module and initialize the environment with the following commands:

```bash
module load miniconda3
source $EBROOTMINICONDA3/etc/profile.d/conda.sh
```
### Miniconda Features
- For Bunya users, Miniconda behaves similarly to the Anaconda module.
- Installing packages with Miniconda may require downloading additional files
  compared to Anaconda, but this difference is usually negligible.
- Miniconda uses the `defaults` channel by default.

---
# Conda Environments
Conda environments (not tp be confused with [Conda modules](#conda-modules)) are
isolated spaces that contain the software and dependencies needed to run your
Python applications.

Once a Conda module (e.g., Miniconda) is loaded and initialized, you can
activate, deactivate, create, delete and modify Conda environments.

## Configuring Default File Locations
By default, Conda stores environments and downloaded package files in your home
directory (`$HOME/.conda/`). These files can quickly consume a significant amount of storage,
especially when working with machine learning libraries or GPU-enabled
packages. Since `/home` has limited quota on Bunya, it is recommended to store
both Conda environments and the package cache in `/scratch/user/<username>`.

To change the default location update Conda's configuration edit `~/.condarc`
and ensure it contains these lines:
```yaml
envs_dirs:
  - /scratch/user/<username>/conda/envs

pkgs_dirs:
  - /scratch/user/<username>/conda/pkgs
```

After making these changes:
- New environments will be created in /scratch/user/<username>/conda/envs.
- Downloaded package files will be stored in /scratch/user/<username>/conda/pkgs.

> [!Note]
> Any existing environments and cached packages in `$HOME/.conda/` will remain there
> unless they are moved manually or recreated in the new location.


## Creating a Conda Environment
By default, environments are created in the
[configured](#configuring-default-file-locations) `envs_dirs`. Typically
$HOME/.conda/envs or /scratch/user/<username>/conda-envs, if configured.

To create a new environment named <my-env>:
```bash
conda create --name <my-env>
```

This creates an empty environment with no installed packages except Conda itself.

To create an environment with a specific Python version, use:
```bash
conda create --name <my-env> python=3.10
```

To create an environment with Python and specific packages, specify them
during creation:
```bash
conda create --name <my-env> python=3.10 numpy scipy pandas
```

> [!Note]
> You can specify versions for packages. For example, `numpy=2.2.3`

To create an environment in a custom location substitute the `--name` flag for
`--prefix` and specify the environment's path:
```bash
conda create --prefix /scratch/project/<some-project>/<some-env>
```
As with the `--name` flag you can also specify python, packages and versions.

More information is available as Conda's
[Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#managing-environments)
documentation.

## Activating an Environment
Activate an environment from your default environment location with:
```bash
conda activate <my-env>
```

You can also activate environments outside your default location by specifying
its full path:
```bash
conda activate /scratch/project/<some-project>/<some-env>
```

After activating an environment Python will use the packages and dependencies
installed within it.

To check which environment is currently active and get
other useful information run:
```bash
conda info
```

## Deactivating as Environment
Deactivate your current environment by running:
```bash
conda deactivate
```
Environments must be deactivated before they can be deleted using Conda



<br><br><br><br><br><br><br><br>

---

# **OLD STUFF**

---



## The Base Environment
If you want to simply run python without installing any packages you may do this
from the base environment. Activate the base conda environment with:<br><br>
`conda activate`<br><br> You can run python and access any packages in the base
environment, but you can't install anything into it. To install packages create
and activate your own environment.


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

