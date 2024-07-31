# Biowulf setup

## 1. `ssh biowulf`

## 2. Start interactive session

[`start-interactive.sh`](https://gist.github.com/michael-ford/f81aa1621a22fa341f6e57b5829d9bb0):
```
sinteractive --mem=40g --cpus-per-task=16 --tunnel --time 36:00:00
```

## 3. SSH into interactive node
`ssh cn4319`

Requires `~/.ssh/config`:
```
Host cn*
        User fordmk
        ProxyCommand /usr/bin/ssh -o ForwardAgent=yes fordmk@biowulf.nih.gov nc -w 120ms %h %p
```

## 4. Start tmux, jupyter:
[`jupyter-tmux.sh`](https://gist.github.com/michael-ford/8264ae57370e6b1da80f81145340623c):
```
#!/bin/bash

port=$(/usr/local/slurm/bin/reconnect_tunnels --list 2> /dev/null)

tmux new-session -d -s jupyter
tmux send-keys -t jupyter "cd /data/$USER; jupyter lab --no-browser --port=$port" C-m

tmux new-session -d -s main
sleep 5
tmux send-keys -t main "cd /data/$USER; get-jupyter-url.sh" C-m
tmux attach -t main

exit 0
```

[`get-jupyter-url.sh`](https://gist.github.com/michael-ford/14f585937217e8bd7e952e8a16971fba):
```
#!/bin/bash

result=""

while [[ -z "$result" ]]; do
    result=$(jupyter server list 2> /dev/null | grep http | cut -f1 -d' ')
    sleep 1
done

echo "$result"
```

## 5. Setup proxy tunnel, open jupyter

`ssh biowulf.nih.gov /usr/local/slurm/bin/reconnect_tunnels`

Copy the URL from the output and paste it into your browser.

## 6. [Setup Visual Studio Code](https://hpc.nih.gov/apps/vscode.html)

- Command + shift + p: `Remote-SSH: Connect current window to host...`
- `cn4319`


# Project Organization

## [Data Science Development Project Template](https://github.com/michael-ford/data-science-development-project-template)

```
├── data
│   ├── external                        <- Data from third party sources.
|   |   └── SOURCE.tsv                  <- Record of data sources
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
```

# Git

## Branching Strategy

### Main Branches
- `main`: Always stable and deployable.
- `develop`: Integrates features for the next release.

### Supporting Branches
- `feature/*`: New features or enhancements.
- `release/*`: Prepares for a new production release.
- `hotfix/*`: Quick fixes for production issues.

### Workflow
1. Create a branch from `develop` for new features.
2. Commit changes with meaningful messages.
3. Regularly sync with `develop`.
4. Push the branch and open a PR for review.
5. After approval, merge the PR and delete the feature branch.
6. Use CI tools to ensure code quality and test coverage.


# Conda

## [Cheatsheet](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html)

## Setup Mamba:

```
conda update -n base conda
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

## Keep track of current state, pip

[Script in `data-science-development-project-template`](https://github.com/michael-ford/data-science-development-project-template/blob/31decde75ed472d132a49b14b6a84c3e039c0d70/%7B%7B%20cookiecutter.repo_name%20%7D%7D/%7B%7B%20cookiecutter.conda_environment_name%20%7D%7D_export_env.py) to export env to `environment.yml`


Example:

```
name: bakir_analysis
channels:
  - bioconda
  - conda-forge
  - defaults
dependencies:
  - nextflow
  - numpy
  - pandas
  - python=3.9
  - ipykernel
  - biopython
  - panel
  - pyfastx
  - yaml
  - parasail-python
  - mappy
  - dill
  - matplotlib
  - seaborn
  - clustalw
  - papermill
```

## Create environment

`conda env create -f environment.yml`

# Containers

## [Example Dockerfile](https://github.com/michael-ford/bakir/blob/aae1d4232d381782f87ae57a35b58b5aca8cbcf0/Dockerfile)

```
FROM continuumio/miniconda3

# Set the working directory in the container
WORKDIR /app

# Install git
RUN apt-get update && apt-get install -y git

# Clone the repository
RUN git clone https://github.com/michael-ford/bakir.git

# Create the conda environment
RUN conda env create -f /app/bakir/bakir-env.yml

# Initialize conda in bash shell
RUN conda init bash

# Make RUN commands use the new environment:
SHELL ["conda", "run", "-n", "bakir", "/bin/bash", "-c"]

# Install bakir within the conda environment
RUN pip install /app/bakir

# Ensure commands run inside the conda environment
ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "bakir", "bakir"]
```

## Host on [Docker Hub](https://hub.docker.com/orgs/cdslsahinalp/members)

Configure automated builds on Docker Hub to build the image when the repository is updated.

## Using Singularity

```
singularity pull docker://cdslsahinalp/kir-annotator:latest
```

# Nextflow

## [Nextflow Documentation](https://www.nextflow.io/docs/latest/index.html)

## [On Biowulf](https://hpc.nih.gov/apps/nextflow.html)

## [nf-core](https://nf-co.re/pipelines/)

