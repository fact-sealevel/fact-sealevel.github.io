# FACTS Output Types

Different steps of a FACTS experiment generate different types of projections. The key element of the FACTS framework is that any module can be run in a given step so long as it ingests and returns the expected objects. For example, a module run in the climate step must produce an Xarray `xr.Dataset` object that looks like `ClimateOutput`, described below. On this page, you'll find a description of each type and references to provided functions to read and write these objects that you can adopt in your module.

## Climate Projections (`ClimateOutput`)

## Global mean sea-level (GMSL) Projections (`GMSLOutput`)

## Relative sea-level (RSL) Projections (`RSLOutput`)

## Extreme sea-level (ESL) Projections (`ESLOutput`)

