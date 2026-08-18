---
icon: lucide/rocket
---

# Welcome! :wave:

Welcome to FACT-Sealevel! This is the home page for the Framework for Assessing Changes to Sea-level 2 (FACTS2) project. On this site, you will find an overview of FACTS, including links to different components of the FACTS ecosystem as well as tutorials and documentation for both contributors and users. 

!!! warning 

    🚧 🚧 🚧   
    This is documentation is still in development. Please check back frequently for updates. If you run into questions or issues, raise an [issue](https://github.com/fact-sealevel/fact-sealevel.github.io/issues).  
    🚧 🚧 🚧
     

---

## What is FACTS?
FACTS2 is an open-source software ecosystem that provides a unified framework for computing and examining projections of global mean, regional, and extreme sea-level change and its uncertainties. It is designed so users can easily explore deep uncertainty by producing and comparing multiple probability distributions that represent different choices of contributing processes. FACTS2 is a refactoring of the original facts ([Kopp et al. 2023](https://gmd.copernicus.org/articles/16/7461/2023/)) codebase into a more loosely coupled set of scientific modules and framework software.

FACTS2 is made up of a suite of independent, command-line applications (or, "modules") that can be executed directly using uv or using docker run to run inside its accompanying Docker container (recommended). Each module application is a GitHub repository and includes a README with instructions for accessing input data and running the module, and includes a working example. Links to the modules are listed in the Module details section below.

Typically, researchers link these modules together in specific ways to estimate distributions of sea level change. The linked modules are known as a FACTS experiment.


## How to use this page

<div class="grid cards" markdown>

-   :lucide-book-open:{ .lg .middle } __About FACTS__

    ---

    Start here to learn about the basics of FACTS such as modules and experiments.
    
    [:octicons-arrow-right-24: FACTS Ecosystem Overview](about_facts/index.md)

-   :lucide-package-plus:{ .lg .middle } __Contribute a module to FACTS__

    ---

    Tutorials and documentation for adding a new module to the FACTS2 module ecosystem

    [:octicons-arrow-right-24: Module Contributor Guide](module_contributor_docs/index.md)

-   :lucide-component:{ .lg .middle } __`fact-sealevel` Overview__

    ---

    All components of FACTS2 are located in the fact-sealevel GitHub organization. Learn about the different components of the FACTS2 ecosystem here.

    [:octicons-arrow-right-24: fact-sealevel overview](fact_sealevel_overview/index.md)
    ---
    
-   :lucide-circle-arrow-out-up-right:{ .lg .middle } __Getting started as a FACTS user__

    ---

    Want to use FACTS to configure and run sea-level experiments? Check out our getting started guide.

    [:octicons-arrow-right-24: Getting Started](getting_started/index.md)

---

</div>

## FACTS Community
The FACTS community is made up of scientists, practitioners, and developers all working on different parts of the FACTS ecosystem. 

### FACTS Slack Workspace
If you haven't yet, consider joining our Slack workspace! 

### GitHub
FACTS is entirely open-source. All code that is a part of the FACTS2 ecosystem is located within the FACT-Sealevel GitHub organization. If you run into any issues with a specific module, please raise an issue in that module's repository. Similarly, you can always raise an issue or start a discussion in the [FACTS-experiment-builder](https://github.com/fact-sealevel/facts-experiment-builder), [FACTS-module-registry](https://github.com/fact-sealevel/facts-module-registry) or [FACTS-experiment-catalog](https://github.com/fact-sealevel/facts-experiment-catalog) repositories. 
