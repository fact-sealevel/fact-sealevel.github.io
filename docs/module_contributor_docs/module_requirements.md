# Requirements of a FACTS2 module

This document describes the set of requirements of a module in the FACTS2 ecosystem. Adding a module to the FACTS2 ecosystem means that it can be used by [facts-experiment-builder](https://github.com/fact-sealevel/facts-experiment-builder), making it easier for others to configure experiments that include your module. The purpose of this document is to help guide new module contributors through the module development process. Other helpful resources include the example module [tutorial](build_facts_module_tutorial.md) and module registry submission [tutorial](submit_module_to_fmr_tutorial.md). If you haven't already, review the [module templtae](https://github.com/fact-sealevel/module-template) provided in the fact-sealevel GitHub organization. The template comes pre-populated with some of the files required for all modules.

----------

## 1. Modules must have an associated GitHub repository
- Ideally, the repo is located in the fact-sealevel GitHub org, but if you prefer, it can be located elsewhere.
    - If elsewhere, the repository must be public and it must have permissions for GitHub Packages, which you can read more about here. If you’re unsure about any of this, please reach out to us! We are happy to help figure this part out.

## 2. The module must be a command line application 
- The modules currently in the FACTS2 ecosystem use Click, which we recommend. However, it is also feasible to use built-in argparse methods if that is your preference (bit about the differences here).
- If you already have a way of creating and organizing an application that works for you, that is great and you should use what is easiest for you. If you haven’t developed a CLI application or would like some guidance, we will share an example/template that we hope will help guide you through the steps of setting up the module (so you can focus on the science!) (this is in progress)

## 3. Module must have a Docker container image
- Module contains a Dockerfile with instructions to build a Docker container image of the application. We are happy to help with this step and the FACTS documentation will have instructions for this + an example soon. 

## 4. Input data must be archived and publicly available (such as a Zenodo record)
- We’re happy to create and submit a Zenodo record for your module input data in the fact-sealevel Zenodo community. If you’d rather archive the data yourself or use something other than Zenodo, that is also fine as long as it has a publicly accessible DOI.
    - Accompanying module input data, there must be instructions to generate that input data. We recommend using this template, but exact guidance is still being developed.

## 5. Module must have an entry in the facts module registry
- This means a directory in the registry repo with the module’s name. Directory contains a module yaml (ie. `module_name_module.yaml`) file. 
- Module yaml must match the schema used by existing modules in the registry.
    - Note: we are working on specific documentation to help with this and will make an empty module yaml that contributors can copy paste as a starting point available soon.

## 6. The module accepts standard forcing inputs and produces standard outputs 
See FACTS Output Types [page](facts_projection_objects.md)
