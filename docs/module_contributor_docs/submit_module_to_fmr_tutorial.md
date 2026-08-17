# Tutorial: Making a `gopher-buckets` entry in the FACTS-module-registry

This tutorial demonstrates how to write an entry in the [facts-module-registry](https://github.com/fact-sealevel/facts-module-registry) for a new module (If you're not familiar with the module registry, we recommend checking out the FACT-sealevel Overview [page](../fact_sealevel_overview/index.md)). We'll continue the example module we used in the previous [tutorial](build_facts_module_tutorial.md) and write an entry to add the gopher-buckets module to the FACTS-module-registry.

An entry in the module registry is a directory containing a single YAML file. The YAML file functions as a 'calling card' of the module including the versioned container image holding the module software, and information about input files and arguments, any files written by the module, and how it interacts with other modules in the FACTS2 ecosystem. 

## Preparation
Before writing a module YAML, we need the following pieces in place: 
- A container image of the working module CLI application. (ie. the end point of the previous [tutorial](build_facts_module_tutorial.md) )

