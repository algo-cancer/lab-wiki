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
