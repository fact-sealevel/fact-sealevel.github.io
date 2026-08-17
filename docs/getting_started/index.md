---
icon: lucide/circle-arrow-out-up-right
---
# Getting started

If you are a user who would like to configure and run sea-level experiments in FACTS2, start here! 

On this page, you will find instructions for everything you need to get started running FACTS experiments.

## Download input data
- FACTS experiments are composed of modules, each module has associated input data. See [downloading input data](downloading_input_data.md) for detailed instructions and code you can copy & paste to download module input data. 

## Install software

- There are various pieces of software that may be required depending on how you plan to use FACTS. Check out our [installation instructions](installation_instructions.md) for more information.

## Decide where you will run FACTS experiments
- FACTS2 is designed to be a flexible framework that can be used across multiple system architectures and run environments. You can run FACTS on a laptop, however, because of the large volume of input data and computational requirements of some modules, you may prefer to run FACTS on an high-performance computing cluster. 
    
### i. Run experiments with Docker Compose
- If you are working on a system with Docker installed, the simplest option is to run an experiment that is structured as a Docker Compose. The FACTS-experiment-builder (FEB) package offers an easy command to generate a [Docker Compose](https://docs.docker.com/compose/) file from a FACTS experiment configuration file. If Docker is not yet installed on your machine, follow the installation instructions [here](installation_instructions.md#docker).

### ii. Run experiments with Apptainer
- Apptainer is an alternative to Docker that is compatible with most HPC systems. FEB does not yet offer an Apptainer implementation similar to Docker Compose. However, it is possible to write a Bash script that replicates the logic of an experiment docker compose file using Apptainer instead of Docker. See our example [here](). 

## See also
- [Installation Instructions](installation_instructions.md)
