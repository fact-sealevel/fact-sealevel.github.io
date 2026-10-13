# Running on HPC

🚧 This page is under construction. Check back soon for updates. 🚧

Because of the large volume of input data and the computational requirements of some modules, you may prefer to run FACTS experiments on a high-performance computing (HPC) cluster. Most HPC systems do not allow Docker, but many support [Apptainer](https://apptainer.org/), which can run the same module container images.

## Current status

FEB currently generates only Docker Compose files (`feb generate-compose`). An Apptainer implementation is under active development [here](https://github.com/fact-sealevel/facts-experiment-builder/tree/add_apptainer_command).

In the meantime, you can still use FEB on a cluster for every step up to and including configuring your experiment (steps 1–6 of [Getting started](index.md)). To run the experiment, you can write a Bash script that reproduces the logic of the generated `experiment-compose.yaml` using `apptainer run` in place of each Docker service. The compose file lists each module's container image, command, arguments, and mounted directories, in the order they need to run.

## See also

- [Installation: Apptainer](installation_instructions.md#apptainer)
- [Experiment compose file overview](https://github.com/fact-sealevel/facts-experiment-builder/blob/main/docs/EXPERIMENT-COMPOSE-OVERVIEW.md)
