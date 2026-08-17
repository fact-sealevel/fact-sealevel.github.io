# Installation

This page has installation instructions for different pieces of software that you may need while running FACTS experiments.

## FACTS-experiment-builder (FEB)
FEB is a package from the FACTS project. It is a command line interface (CLI) tool designed to streamline defining FACTS experiments and generating execution scripts to run experiments. Find the installation guide [here](https://github.com/fact-sealevel/facts-experiment-builder/blob/main/README.md#installation).

## Docker
If you choose to run experiments via Docker Compose, Docker must be installed on your machine. Follow Docker's installation instructions [here](https://docs.docker.com/get-started/get-docker/). 

## Apptainer
An alternative to Docker, Apptainer only runs on Linux machines. Follow the Apptainer installation instructions [here](https://apptainer.org/docs/user/main/quick_start.html#installation).

## Python package manager
This will be necessary to install and run the FACTS-experiment-builder (FEB) package. We recommend using [uv](https://docs.astral.sh/uv/) but [pip](https://pypi.org/project/pip/) or [pipx](https://pipx.pypa.io/stable/) would work as well if you prefer them. See uv installation instructions [here](https://docs.astral.sh/uv/getting-started/installation/).

## git
Git is used by a command in FEB to clone the FACTS-module-registry to your FACTS workspace. You can find installation instructions [here](https://github.com/git-guides/install-git).