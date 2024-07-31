## Conda Environments

We use Conda to manage our Python environments. If you’re familiar with pip, this is similar. Conda is a package management system that enables you to create and manage separate environments for different projects. Here are some of the top benefits of using Conda:

- **Isolation:** Conda allows you to create and manage isolated environments for different projects, meaning that you can have different versions of Python and different packages installed for different projects without them conflicting with one another.
- **Reproducibility:** By specifying the packages and their versions in your environment file, you can ensure that all team members are working with the same dependencies and versions. This makes it easy to reproduce your development and analysis environments.
- **Ease of use:** Conda provides a user-friendly interface for creating, managing, and sharing environments. You can easily create a new environment with the packages you need, activate it, and start working.
- **Cross-platform compatibility:** Conda works on Windows, macOS, and Linux, so you can easily share your environment files with others regardless of their operating system.

### Installation

1. Run `echo /home/$USER/data/miniconda3/` to get the install location for conda. Copy the result
2. [Download the Miniconda installer](https://docs.conda.io/en/latest/miniconda.html) and run
3. Specify your copied install location when prompted in the installer
4. DO NOT say yes to option to run conda init / copy code to .bashrc
5. After install, copy the following code to your `~/.bash_profile`

```bash
source /home/$USER/data/miniconda3/etc/profile.d/conda.sh
export PATH="/home/$USER/data/miniconda3/bin:$PATH"
conda activate base
```

1. Start a new shell and make sure it works
2. Now install the mamba solver by running

```bash
conda update -n base conda
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

### Tracking your environment for reproducibility

It is critical to keep track of what is installed in your environment so that your work is reproducible. We do this by maintaining a `yml` file containing all the packages that need to be installed.

<aside>
💡 A default environment is provided when you configure the cookiecutter *data-science-project-development-template\*
</aside>


## Setting up Conda envs as jupyter kernels

We have abandoned `nb_conda_kernels` because it is bloated and no longer maintained. Below is the method for managing conda kernels in jupyter without it.

Lets assume you have conda installed with a base env, which contains jupyter, and another env, called `myenv`that youd like to use as a kernel in jupyter

1.  First, ensure ipykernel is installed in your env:

```jsx
(base) $ conda install -n myenv ipykernel
```

1.  Get location of conda install:

```jsx
(base) $ prefix=$(which python | awk -F'/' '{OFS="/"; $NF="/"; $(NF-1)=""; print $0 }' | sed 's/\/\/*$//')
```

Copy everything before `bin`

1.  Now lets install the kernel:

```jsx
(base) $ conda activate myenv
(myenv) $ python -m ipykernel install --name myenv --display-name "python3 (myenv)" --prefix $prefix --env PATH $PATH
[InstallIPythonKernelSpecApp] WARNING | Installing to /data/<user>/miniconda3/share/jupyter/kernels, which is not in ['/data/<user>/miniconda3/envs/kir/share/jupyter/kernels', '/spin1/home/linux/<user>/.local/share/jupyter/kernels', '/home/<user>/.local/share/jupyter/kernels', '/usr/local/share/jupyter/kernels', '/usr/share/jupyter/kernels', '/spin1/home/linux/<user>/.ipython/kernels']. The kernelspec may not be found.
Installed kernelspec <myenv> in /data/<user>/miniconda3/share/jupyter/kernels/kir
```

Dont worry about the error!

1. Lets check it worked:

```jsx
(myenv) $ conda deactivate
(base) $ jupyter kernelspec list
Available kernels:
  myenv        /data/<user>/miniconda3/share/jupyter/kernels/kir
  python3    /data/<user>/miniconda3/share/jupyter/kernels/python3
```

Yay! **If you already have jupyter running (in base) youll have to refresh the page**

## Removing kernels

is simple:

```jsx
(base) $ jupyter kernelspec remove myenv
```