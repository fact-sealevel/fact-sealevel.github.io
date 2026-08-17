# `fact-sealevel` Overview

## Components of the FACT-Sealevel organization

### FACTS-experiment-builder (FEB)

The FEB package contains CLI tools to support writing and executing experiments in FACTS. Experiments are centered around artifacts called `experiment-configuration.yaml` files. The experiment config file is the full scientific specification of an experiment (for more detail on FACTS experiments, see [FACTS experiments](../about_facts/facts_experiment.md) overview page). It is possible to write an experiment config file by hand, however, FEB offers a CLI command, `setup-experiment` that automatically generates a configuration file based on user inputs. 

As mentioned above, the experiment configuration file is just an artifact, it is not an executable script to run an experiment. FEB contains another CLI command, `generate-compose`, that produces a Docker Compose file that is a direct implementation of the corresponding `experiment-config.yaml` file. 

### FACTS2 module ecosystem
Modules are the core units of FACTS. Modules represent physical processes related to sea-level change. FACTS experiments are assemblages of FACTS modules in different combinations and with set parameterizations. 

All modules are stand-alone, containerized command line applications, meaning that they can be run on their own as well as part of a FACTS experiment. FACTS2 modules are found in the FACT-sealevel GitHub organization [repositories](https://github.com/orgs/fact-sealevel/repositories) section. The module ecosystem interfaces with the FEB package through the FACTS-module-registry. 

### FACTS module registry
The FACTS module registry is another repository within the FACT-sealevel GitHub organization. The registry contains an entry for each module in the ecosystem. This registry entry is the piece of information that allows the expermint builder package (FEB) to communicate with modules in the ecosystem. Entries in the FACTS module registry consist of a directory named after the module and containing a module yaml file. This yaml functions as the 'calling card' for the module, containing information about its inputs, outputs and parameters, etc. 

An entry in the moduel registry consists of a directory in the repo matching the name of a module. The directory holds a module yaml file. For example, the module registry entry for the module [ssp-landwaterstorage](https://github.com/fact-sealevel/ssp-landwaterstorage) is a directory called 'ssp_landwaterstorage' holding a file named [ssp_landwaterstorage_module.yaml](https://github.com/fact-sealevel/facts-module-registry/blob/main/ssp-landwaterstorage/ssp_landwaterstorage_module.yaml)

FEB uses a module's registry entry when creating an experiment configuration file (`feb setup-experiment`) and an experiment compose file (`feb generate-compose`). In addition, `feb check-data` uses the module YAML files to check that required module input data are downloaded locally.

For more information on what is in the module yaml and how to write one, see the 'Submitting a module to the FACTS Module Registry' [tutorial](../module_contributor_docs/submit_module_to_fmr_tutorial.md)