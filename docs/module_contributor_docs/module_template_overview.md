# Module Template Overview

The FACTS project provides a GitHub template repository that can be used to build repositories for new FACTS modules. This template contains a number of files to help get started when creating a new module (for a tutorial on making a new module, ) check out the 'Build a FACTS Module' [tutorial](build_facts_module_tutorial.md). On this page, you'll find an overview of each piece of the module template and what it does. 

## src/module_template
This is the codebase of your module. It holds Python scripts. **Important:** Must also have a `__init__.py` file.

## .dockerignore
Similar to `.gitignore`, files to be ignored when creating the Docker container image

## .gitignore
Files to ignore in this repository for git tracking

## .python-version
Specifices the Python version used in this project.

## CHANGELOG.md
Used for tracking changes associated with this module. We recommend the [Keep a changelog](https://keepachangelog.com/en/1.1.0/) approach for maintaining organized, readable changelogs. 

## README.md
Your module's about page. We recommend following the example laid out in the module template and other modules in the FACTS2 module ecosystem. 

## justfile
This is used to run linting, formatting, tests and other automatic processes. 

## pyproject.toml
The main file defining your project. It is created by running `uv init` but comes pre-populated when you create a new repository from the module template. 

## uv.lock
A lock file for your project's dependencies.