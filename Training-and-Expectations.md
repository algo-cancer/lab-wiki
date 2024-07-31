# Tumor Evolution and Genomics Section Training and Expectations Outline

Owner: Zlatko Salko Lagumdzija, Michael Ford
Verification: Expired
Tags: Onboarding

# Background training

Its expected that you are familiar **all** of the following skills/tools. **If any of these are new to you, or you’ve only had light exposure in the past, now is the time to brush up** by reading the relevant chapters in [Bioinformatics Data Skills](https://drive.google.com/file/d/1rNSQil1CxVK9hW2-TzDf5De3xyhpY9SE/view?usp=sharing). 

- bash command line
    - File manipulation
    - grep
    - sed
    - cut
    - find
    - less
    - piping and redirecting
- tmux
- git
    - adding, commiting
    - branching, merging, rebasing/squashing
    - github: push, pull, fetch
- types of sequence information
    - fasta, fai index files
    - fastq
    - bam/sam
    - bed files
- mapping sequencing data with [BWA](https://www.notion.sobwa/)
- sam/bam manipulation with samtools and pysam

# Workflow

The following is tailored to python, but if you are using other languages you should read through to take away the general principles.

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

## Jupyter notebooks

Use these for:

1. Prototyping code (strongly recommend also having a .py file open to transfer your code to)
2. High level running of experiments
3. Visualizing data / results

**Beware that it is really easy to end up with a messy and convoluted notebook** if you do not regularly transfer code out of the notebook into a separate .py that you import, **especially if you run cells not in order**.

See the ‘Setting up Jupyter Headless’ page for how to use jupyter on biowulf

## Reproducible data and experiments

It is critical that every command or pipeline you run to produce any results, whether that is fetching raw data, generating interim data, or producing figures and results. To do this, we strongly recommend using [Nextflow](https://nextflow.io/), as it is currently the most popular and growing pipeline management tool (and a requirement for most industry bioinformatics jobs).

Note: It is important that you use the newer **DSL2 format**.

Organize your Nextflow code as you would your python code:

- Save modular processes in `.nf` files in the `src` directory
- For every pipeline run, make a new `.nf` file where you import the required processes from `src` and put them together in the `workflow`. Input and output files should be hardcoded in this script, rather than specific as command line arguments.

Here is an example of fetching a paired end wgs sequence, mapping using BWA and sorting and indexing using samtools:

```jsx
// File: src/data/wgs_mapping_processing.nf
nextflow.enable.dsl=2

process bwa_mapping {
    input:
    path ref_genome
    path read1
    path read2

    output:
    path "output_mapped.bam", emit: bam

    script:
    """
    bwa mem $ref_genome $read1 $read2 > output_mapped.bam
    """
}

process sort_bam {
		publishDir './', mode: 'rellink'

    input:
    path bam

    output:
    path "output_sorted.bam", emit: sorted_bam

    script:
    """
    samtools sort $bam -o output_sorted.bam
        rm $bam  // Removes the input BAM once the sorted BAM is created to save space
    """
}

process index_bam {
		publishDir './', mode: 'rellink'

    input:
    path sorted_bam

    output:
    path "output_sorted.bam.bai", emit: bam_index

    script:
    """
    samtools index $sorted_bam
    """
}
```

This is your definition of you processes in a modular format located in `src/data`

```jsx
// File: data/raw/sample_A_wgs/fetch_map_sample_A_wgs.nf
nextflow.enable.dsl=2

include { bwa_mapping; sort_bam; index_bam } from '../../../src/data/wgs_mapping_processing.nf'

params.ref_genome = '../../external/genome-references/GRCh38.fasta'

process fetch_fastqs {
    output:
    path "sample_A_R1.fastq", emit: read1
    path "sample_A_R2.fastq", emit: read2

    script:
    """
    curl -O http://example-dataset-portal/sample_A_R1.fastq
    curl -O http://example-dataset-portal/sample_A_R2.fastq
    """
}

workflow {
    fetch_fastqs()

    bwa_mapping(params.ref_genome, fetch_fastqs.out.read1, fetch_fastqs.out.read2)
    sort_bam(bwa_mapping.out.bam)
    index_bam(sort_bam.out.sorted_bam)

    }
```

This is the actual script that gets run to process the data. Note the following features:

- Process `FETCH_FASTQS`: This could be also written as a modular process in `src`, but since it is very simple, and there are many different ways of fetching data from a database, it is sufficient to have it defined here. If you use this specific method many times for different applications, consider moving it to `src` as a modular process to reduce code duplication
- The `include` statement to import the relevant processes
- The `workflow` stitches the processes together into a pipeline

What you end up with in `data/raw/sample_A_wgs` after running `nextflow fetch_map_sample_A_wgs.nf`

- `fetch_map_sample_A_wgs.nf`
- `work` directory
- `output_sorted.bam -> work/some/hash/output_sorted.bam`
- `output_sorted.bam.bai -> work/some/other_hash/output_sorted.bam.bai`

By then commiting `fetch_map_sample_A_wgs.nf`, and the symlinks `output_sorted.bam` and `output_sorted.bam.bai` you ensure that any future reproductions of this workflow with be identical (otherwise you will notice the symlinks have changed when you run `git status` as they will point to new locations)

<aside>
💡 **Use ChatGPT to help write you .nf scripts!!**

</aside>

For example, the above was generated using the following prompts:

> Please provide two nextflow scripts, written with DSL2, the first being "wgs_mapping_processing.nf", which contains 3 processes: 1) mapping two input 2 fastq files representing paired end reads to an input genome reference using bwa and saving to a bam file, 2) sorting the resultant bam file, 3) indexing the resultant bam file. The second script, called "fetch_map_sample_A_wgs.nf" includes a process to fetch two fastq files from "http://example-dataset-portal" using curl, and then imports the processes defined above from "../../../src/data/wgs_mapping_processing.nf' and runs the processes in the workflow using the downloaded fastqs. The output bam file should be called "sample_A_wgs.bam".
> 

And then

> You are an expert in reproducible data analysis, coding best practices and nextflow code standards. Perform a code review of the following nf scripts, and provide improved versions if needed. Note that wgs_mapping_processing.nf is intended to be a collection of modular processes, while fetch_map_sample_A_wgs.nf is intended to be a workflow that is run as single instance of a pipeline and not be modular.
> 

---

# Coding Standards and Best Practices

Since we are a method development group, we need to produce *production level* code. If you have not had much experience with any of the following, it is worth the time investment to get up to speed - not only so your project will be more stable, usable and maintainable, but **to give you skills that future employers want to see, guaranteed.**

## Paradigm: OOP

Object-oriented programming (OOP) powerful and the standard in production development environments. Reminder of the main features and benefits below. Ensure you are integrating the all of the following principles in your code!

- **Modularity and encapsulation:** OOP allows you to break down complex programs into smaller, more manageable pieces, or modules. Each module can have its own data and functionality, making it easier to work on and test in isolation. Encapsulation ensures that the internal workings of a module are hidden from other parts of the program, reducing the likelihood of bugs and making it easier to modify and update the code.
- **Code reuse:** OOP encourages the creation of reusable code, which can save time and effort in the long run. Objects can be created once and used multiple times, reducing the amount of code that needs to be written and maintained.
- **Inheritance and polymorphism:** OOP allows you to create new objects based on existing objects, through inheritance. This can save time and effort in creating new code, and also helps ensure consistency across the program. Polymorphism allows objects to take on multiple forms, depending on the context, which can make code more flexible and extensible. This allows you to easily iterate on your tool development to test different features.
- **Abstraction:** OOP allows you to create abstract data types that capture the essence of a concept, without getting bogged down in implementation details. This can make code more understandable and easier to reason about.

## Typing and Commenting

It is important for your code to be readable and maintainable. Ensure you are integrating all of the following:

- **Commenting**
    - Use comments to explain why code is written in a certain way, not what the code is doing.
    - Avoid commenting every line of code, instead focus on complex or non-obvious sections of the code.
    - Use descriptive names for variables and functions to reduce the need for comments.
    - Use inline comments sparingly and only when necessary.
    - Use a consistent commenting style throughout the codebase.
- **Typing**
    - Use type hints for function arguments, return values, and class attributes.
    - Use Union types to indicate that a function can accept multiple types of arguments.
    - Use Optional types for arguments that are not required.
- **Docstrings**
    - Use docstrings to describe the purpose and behavior of functions and classes.
    - Follow the [Google style guide](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) for writing docstrings.
    - Start the docstring with a one-line summary of the function or class.
    - Include information on function arguments, return values, and exceptions that can be raised.
    - Include examples of how to use the function or class.

💡

Circular imports can be a problem when typing. Use the following methods to avoid it:

1. Using the following type check

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from models import Book
```

1. Use `from __future__ import annotations` and **put your typing in single quotes**

## Code testing

WIP: We need it, write it.