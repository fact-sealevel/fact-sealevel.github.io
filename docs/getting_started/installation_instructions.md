# Installation

This page has installation instructions for the software you may need to run FACTS experiments. For the order in which to set things up, see [Getting started](index.md).

| Software | Needed for | Required? |
|---|---|---|
| [uv](#uv) | Running FEB | Yes |
| [git](#git) | `feb init` (clones the module registry) | Yes |
| [FACTS-experiment-builder (FEB)](#facts-experiment-builder-feb) | Configuring experiments and generating run scripts | Yes (no install needed; runs via `uvx`) |
| [Docker](#docker) | Running experiments with Docker Compose | If running locally |
| [Apptainer](#apptainer) | Running experiments on HPC systems | If running on HPC |

## uv
[uv](https://docs.astral.sh/uv/) is a Python package and project manager. It is used to run the FACTS-experiment-builder (FEB) package. See the uv installation instructions [here](https://docs.astral.sh/uv/getting-started/installation/).

## git
`feb init` uses git to clone the FACTS-module-registry into your FACTS workspace. You can find installation instructions [here](https://github.com/git-guides/install-git).

## FACTS-experiment-builder (FEB)
FEB is a command line interface (CLI) tool from the FACTS project that streamlines defining FACTS experiments and generating scripts to run them.

You don't need to install FEB. Instead, run it directly from its GitHub repository with [`uvx`](https://docs.astral.sh/uv/guides/tools/), which comes with uv:

```shell
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb <command>
```

!!! Note "Why not install FEB?"
    FEB is under active development and is updated frequently, and it is not yet published on [PyPI](https://pypi.org/). Running it with `uvx` from GitHub means you always use recent code rather than an installed copy that quietly goes out of date. This cumbersome workaround will change once FEB is published on PyPI: you will then be able to install it and run a plain `feb` command.

### Getting the latest version
`uvx` caches FEB after the first run, so it does not check GitHub for updates every time. To make sure you are using the latest version, add `--refresh` to your first command of a session:

```shell
uvx --refresh --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb --help
```

See the FEB [CHANGELOG](https://github.com/fact-sealevel/facts-experiment-builder/blob/main/CHANGELOG.md) for what has changed between releases.

### If you previously installed FEB
If you installed FEB earlier with `uv tool install`, `pipx` or `pip`, we recommend uninstalling it so that you don't accidentally run an old copy by typing `feb`:

```shell
uv tool uninstall facts-experiment-builder   # or: pipx uninstall facts-experiment-builder / pip uninstall facts-experiment-builder
```

## Docker
If you choose to run experiments via Docker Compose, Docker must be installed on your machine. Install Docker Desktop (macOS/Windows) or Docker Engine (Linux) by following Docker's installation instructions [here](https://docs.docker.com/get-started/get-docker/).

## Apptainer
Apptainer is an alternative to Docker that is available on most HPC systems. It runs only on Linux. On a shared cluster it is usually already installed; check with your system administrators. Otherwise, follow the Apptainer installation instructions [here](https://apptainer.org/docs/user/main/quick_start.html#installation). See [Running on HPC](running_on_hpc.md) for how to use it with FACTS.
