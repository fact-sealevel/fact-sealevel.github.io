---
icon: lucide/circle-arrow-out-up-right
---
# Getting started

If you are a user who would like to configure and run sea-level experiments in FACTS2, start here!

This page walks you through everything you need to go from a fresh machine to a completed FACTS experiment. By the end, you will have a FACTS workspace on your machine, input data for the modules you want to run, and the output of your first experiment.

!!! Note
    FACTS2 and the FACTS-experiment-builder (FEB) are under active development. If you run into problems, please open an issue in the [facts-experiment-builder repository](https://github.com/fact-sealevel/facts-experiment-builder/issues).

**The workflow at a glance:**

0. [Decide where you will run FACTS](#0-decide-where-you-will-run-facts)
1. [Install the required software](#1-install-the-required-software)
2. [Initialize a FACTS workspace](#2-initialize-a-facts-workspace) with `feb init`
3. [Choose your modules](#3-choose-your-modules) with `feb list-modules`
4. [Download input data](#4-download-input-data)
5. [Check your input data](#5-check-your-input-data) with `feb check-data`
6. [Configure an experiment](#6-configure-an-experiment) with `feb setup-experiment`
7. [Generate and run the experiment](#7-generate-and-run-the-experiment) with `feb generate-compose`

---

## 0. Decide where you will run FACTS

FACTS2 is designed to be a flexible framework that can be used across multiple system architectures and run environments. This choice determines which software you need to install in the next step.

- **Your own machine, with Docker (recommended for getting started).** FEB generates a [Docker Compose](https://docs.docker.com/compose/) file that runs your whole experiment with a single command. This works on macOS, Windows, and Linux.
- **A high-performance computing (HPC) cluster, with Apptainer.** Some modules have large input data and heavy computational requirements, so you may prefer to run FACTS on a cluster. Most HPC systems do not allow Docker but do support [Apptainer](https://apptainer.org/). A FEB command to generate a script that runs a FACTS experiment using Apptainer is in progress and can be viewed [here](https://github.com/fact-sealevel/facts-experiment-builder/tree/add_apptainer_command). See [Running on HPC](running_on_hpc.md) for the current options.

The rest of this page follows the Docker path.

---

## 1. Install the required software

You will need:

- **[uv](https://docs.astral.sh/uv/getting-started/installation/)**, which runs FEB.
- **git**, which `feb init` uses to clone the FACTS module registry.
- **Docker** (or **Apptainer**, if you are running on HPC).

You don't need to install FEB itself. Instead, you run it directly from its GitHub repository with [`uvx`](https://docs.astral.sh/uv/guides/tools/), which downloads and runs FEB on demand. Confirm that it works:

```shell
uvx --refresh --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb --help
```

!!! Note "Why the long command?"
    FEB is under active development and is updated frequently, and it is not yet published on [PyPI](https://pypi.org/). Until it is, every FEB command on this site starts with `uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main`, which runs FEB straight from the latest code on GitHub. This avoids ending up with an outdated installed copy. Once FEB is published on PyPI, this will be replaced with a simple install and a plain `feb` command.

    `uvx` caches FEB after the first run. When you come back to FACTS after a break, add `--refresh` (as in the command above) to your first command to make sure you have the latest version.

See [Installation](installation_instructions.md) for links to installation guides for each tool.

**✓ Checkpoint:** the command above (`uvx --from ...`) prints a list of FEB commands, and `docker --version` prints a version number.

---

## 2. Initialize a FACTS workspace

A FACTS workspace is a directory that holds your experiments and a copy of the [FACTS module registry](../fact_sealevel_overview/fmr/index.md). Create a directory wherever you would like to keep your FACTS work, move into it, and run `feb init`:

```shell
mkdir facts-workspace
cd facts-workspace
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb init
```

This command:

1. Creates an `experiments/` subdirectory to hold your experiments and their outputs.
2. Clones the [facts-module-registry](https://github.com/fact-sealevel/facts-module-registry) into your workspace.
3. Adds `facts-module-registry/` to a `.gitignore` file (creating it if needed), so that the cloned registry isn't accidentally committed if you put your workspace under version control.
4. Writes a `.facts-workspace` marker file.

It is safe to re-run `feb init` on an existing workspace; existing files will not be overwritten.

**✓ Checkpoint:** your workspace looks like this:

```
facts-workspace/
├── .facts-workspace           # marker file written by feb init
├── .gitignore                 # ignores facts-module-registry/
├── experiments/               # your experiments will go here
└── facts-module-registry/     # cloned from GitHub
    ├── fair-temperature/
    ├── bamber19-icesheets/
    └── ... (one directory per module)
```

**Run all remaining commands on this page from your workspace root.**

---

## 3. Choose your modules

A FACTS experiment is made up of [modules](../about_facts/facts_module.md) that run in a sequence of steps: a climate step, a sea-level step, a totaling step, and an (optional) extreme sea-level step. See [FACTS experiments](../about_facts/facts_experiment.md) for more on how these steps fit together.

List the modules available in your registry:

```shell
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb list-modules
```

These names are the values you will pass to `feb setup-experiment` in step 6. Decide which modules you want to run now, because each module needs its own input data, and you only need to download data for the modules you use.

!!! Tip
    Not sure where to start? The [facts-experiment-catalog](https://github.com/fact-sealevel/facts-experiment-catalog) contains pre-configured experiments, including configurations used in IPCC AR6.

---

## 4. Download input data

FACTS uses two kinds of input data:

- **Shared input data** is used by many modules (for example, GRD fingerprints and the location file that lists the sites you want projections for).
- **Module-specific input data** is used by only one module (or one family of modules).

Create a `data/` directory in your workspace with one subdirectory for each kind:

```shell
mkdir -p data/shared_input_data
mkdir -p data/module_specific_input_data
```

Then follow the instructions on the [module input data](downloading_input_data.md) page:

1. Download the [shared input data](downloading_input_data.md#shared-input-data).
2. For each module you chose in step 3, copy and run the commands from its section of the [module-specific input data](downloading_input_data.md#module-specific-input-data) list.

!!! Note
    Input data can be stored anywhere on your machine; it does not have to be inside your workspace. If you store it elsewhere, adjust the paths in the following steps.

---

## 5. Check your input data

Before configuring an experiment, confirm that every expected file is in place:

```shell
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb check-data --data-dir data
```

`feb check-data` looks for a subdirectory for each module in `data/module_specific_input_data/`, checks it against the files that module's registry entry expects, and also checks `data/shared_input_data/`. It prints a ✓ or ✗ for each module and lists any missing files:

```
✓ fair-temperature (4/4 files present)
✗ bamber19-icesheets (0/1 files present)
  missing: data/module_specific_input_data/bamber19-icesheets/bamber19_icesheets_preprocess_data.tgz

Shared input data:
✓ shared_input_data (3/3 files present)
```

**✓ Checkpoint:** every module you plan to run shows ✓.

---

## 6. Configure an experiment

An experiment is defined by an **experiment configuration file**, `experiment-config.yaml`. This file is the full scientific specification of the experiment: which modules run, which scenario, which projection years, and so on. It does not run anything on its own.

Create one with `feb setup-experiment`. For example, this creates an experiment called `my_first_experiment` that runs FaIR for the climate step and three sea-level modules under SSP1-2.6. All of these modules have small input data (about 56 MB in total, including the shared data):

!!! Note
    For `--module-specific-iput-data` and `--shared-input-data` paths below, be sure to replace the stand-in text with the paths to each directory on your machine.


```shell
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb setup-experiment \
  --experiment-name experiments/my_first_experiment \
  --climate-step fair-temperature \
  --sealevel-step fittedismip-gris,ssp-landwaterstorage,kopp14-verticallandmotion \
  --extremesealevel-step extremesealevel-pointsoverthreshold \
  --pipeline-id aaa --scenario ssp126 --baseyear 2005 \
  --pyear-start 2020 --pyear-end 2150 --pyear-step 10 \
  --nsamps 1000 --location-file location.lst \
  --module-specific-input-data "path/to/module_specific_input_data" \
  --shared-input-data "path/to/shared_input_data" 
```

A few things to note:

- **Include a parent directory in the experiment name** (here, `experiments/`). This isn't required, but it keeps your workspace organized. If you use a parent directory other than `experiments/`, create it first.
- **Only `--experiment-name` and a climate step are required.** Any fields you don't pass at the command line must be filled in by hand in `experiment-config.yaml` afterwards. Run `uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb setup-experiment --help` to see every option.
- **You may be asked about workflows.** If you include multiple sea-level modules, FEB asks whether you'd like to define additional *workflows*: groups of modules that are summed together. See the [glossary](../about_facts/glossary.md) for more detail.

**✓ Checkpoint:** `experiments/my_first_experiment/experiment-config.yaml` exists. Open it and make sure every required field is filled in. See the [experiment configuration file overview](https://github.com/fact-sealevel/facts-experiment-builder/blob/main/docs/EXPERIMENT-CONFIG-OVERVIEW.md) for a description of each field.

---

## 7. Generate and run the experiment

Generate a Docker Compose file from your experiment configuration:

```shell
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb generate-compose --experiment-name experiments/my_first_experiment
```

This writes `experiment-compose.yaml` next to your `experiment-config.yaml`. It contains everything Docker needs to run each module in the right order. See the [experiment compose file overview](https://github.com/fact-sealevel/facts-experiment-builder/blob/main/docs/EXPERIMENT-COMPOSE-OVERVIEW.md) for details.

Inspect the file, then run the experiment:

```shell
docker compose -f experiments/my_first_experiment/experiment-compose.yaml up
```

**✓ Checkpoint:** when the run finishes, the outputs are in `experiments/my_first_experiment/`.

Congratulations, you have run your first FACTS2 experiment! 🎉

---

## Next steps

- Learn more about [how FACTS experiments are structured](../about_facts/facts_experiment.md).
- Browse pre-configured experiments in the [facts-experiment-catalog](https://github.com/fact-sealevel/facts-experiment-catalog).
- See the [FEB README](https://github.com/fact-sealevel/facts-experiment-builder/blob/main/README.md) for the full reference for each `feb` command, including running with pre-existing climate data in place of a climate module.
- Running on a cluster? See [Running on HPC](running_on_hpc.md).
