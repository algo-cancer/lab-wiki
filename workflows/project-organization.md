## Project organization

The Sahinalp Group uses Cookiecutter Data Science as a guide for organizing data science projects. This structure includes directories such as `data`, `models`, `notebooks`, `references`, `reports`, `src`, and others, and is intended to keep code organized and make it easy to reproduce analyses. The project also includes a `Makefile` with commands to automate tasks. The guiding principles for this structure are:

The guiding principles:

- Keep the project **organized and well-documented**
- Make it easy to **reproduce the analysis**
- Encourage **modularity and reusability of code**
- Use **version control**
- Automate repetitive tasks
- Use a virtual environment and package manager

We have a modified version of cookiecutter data science:

https://github.com/michael-ford/data-science-development-project-template

To use it to start a new project simply run

`cookiecutter https://github.com/michael-ford/data-science-development-project-template.git`

It has the following format:

```
├── data
│   ├── external                        <- Data from third party sources.
|   |   └── SOURCE.tsv                  <- Record of data sources when not using `dvc run`
│   ├── interim                         <- Intermediate data that has been transformed.
│   ├── raw                             <- The original, immutable data dump.
│   └── results                         <- Final results
|
├── environment.yml                     <- Conda environment definition for reproducing
|
├── experiments                         <- Experiments devided by subdirectory
│   └── 03-04-14.sample-experiment      <- Sample demonstrating the structure
│       ├── data -> ../../data/         <- Softlink to data/ for code cleanliness
│       ├── notebooks                   <- Jupyter notebooks for experiment
|       |   └── template.ipynb          <- Sample notebook containing useful code
│       └── results                     <- Final reports for communication (e.g. html
|           |                              version of jupyter nb)
│           └── figures                 <- Figures for reports
|
├── LICENSE
├── README.md                           <- The top-level README for developers using
|                                          this project.
├── setup.py                            <- Configuration for installing src/ as python package
|
└── src                                 <- Source code for use in this project.
    ├── data
    │   └── __init__.py
    └── __init__.py                     <- Makes src a Python module
        |
        └---- tool-repo
```

The key principles are:

1. Code goes in `src`, which is loaded as a python package in develop mode in your [environment](https://www.notion.so/18018d8e695a4fc28b1b62c49c20feba?pvs=21). This allows you to develop your scripts and load them into jupyter notebooks using `from src.data.my_script import my_function`. In order to install it, simply run `python setup.py develop`
2. Experiments, which are generally running of some data and looking at results, are organized in the `experiments` directory. There is another cookiecutter template to create a new experiment directory, which you can run using `cd experiments cookiecutter ../new-experiment-template`
3. Experiment notebooks should set the working directory as the root experiment directory (see `template.ipynb`). This allows them to reference data files using the `data` symlink.
4. Data files are stored in `data` - **which should not be stored in git**. Git works great for plain text files, since it only saves changes to text files in the form of blobs. This does not work for binary or compressed files, as small changes can change the entire file encoding, requiring git to duplicate data, which can blow up the size of your repo. Use a `.gitignore` to not include files in the `data` directory
    1. It is important to keep your data directory organized - sub directories are your friend here. Very little if any data files should be saved directly in `raw`, `external`, `interim` or `results`. Instead they should be saved within a subdirectory, **with a descriptive and informative name,** of the relevant directory.
