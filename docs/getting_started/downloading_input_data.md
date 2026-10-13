# Module input data

This page has copy & paste commands to download input data for modules in the FACTS2 ecosystem. It is step 4 of [Getting started](index.md#4-download-input-data).

Run these commands from your **FACTS workspace root** (the directory where you ran `feb init`). Data is stored under a `data/` subdirectory of the workspace:

```shell
mkdir -p data/module_specific_input_data
mkdir -p data/shared_input_data
```

Download sizes listed below are for the compressed archives (`.tgz` files) you download. Extracted data takes up additional space, and the scripts keep the downloaded archives.

When you're done, confirm that every file is in place (see [step 5](index.md#5-check-your-input-data)):

```shell
uvx --from git+https://github.com/fact-sealevel/facts-experiment-builder@main feb check-data --data-dir data
```

## Shared input data

GRD fingerprint data (download size: 24 MB):
```bash
curl -L https://zenodo.org/record/7478192/files/grd_fingerprints_data.tgz -o data/shared_input_data/grd_fingerprints_data.tgz
tar -xzf data/shared_input_data/grd_fingerprints_data.tgz -C data/shared_input_data
```

Location file (example with New York):
```bash
echo "New_York	12	40.70	-74.01" > data/shared_input_data/location.lst
```

## Module-specific input data

!!! Note
    The examples shared below are a temporary solution. In the near future, we plan to update the instructions and guidelines around downloading module input data. If you are trying to download input data for a module that you don't see listed here, please check the [FACTS-module-registry](https://github.com/fact-sealevel/facts-module-registry); you should find module-specific instructions in each entry as well. 
    
### fair-temperature

Download size: 31 MB (two downloads: 6.8 MB + 24.2 MB)

```bash
mkdir -p data/module_specific_input_data/fair-temperature
curl -L https://zenodo.org/record/7478192/files/fair_temperature_fit_data.tgz -o data/module_specific_input_data/fair-temperature/fair_temperature_fit_data.tgz
tar -xzf data/module_specific_input_data/fair-temperature/fair_temperature_fit_data.tgz -C data/module_specific_input_data/fair-temperature
curl -L https://zenodo.org/record/7478192/files/fair_temperature_preprocess_data.tgz -o data/module_specific_input_data/fair-temperature/fair_temperature_preprocess_data.tgz
tar -xzf data/module_specific_input_data/fair-temperature/fair_temperature_preprocess_data.tgz -C data/module_specific_input_data/fair-temperature
```

### fair2-climate

Download size: 32.8 MB

```bash
mkdir -p data/module_specific_input_data/fair2-climate
curl -L https://zenodo.org/records/18190680/files/fair2_climate_project_data.tgz -o data/module_specific_input_data/fair2-climate/fair2_climate_project_data.tgz
tar -xzf data/module_specific_input_data/fair2-climate/fair2_climate_project_data.tgz -C data/module_specific_input_data/fair2-climate
```

### fittedismip-gris

Download size: 6 KB

```bash
mkdir -p data/module_specific_input_data/fittedismip-gris
curl -L https://zenodo.org/record/7478192/files/FittedISMIP_icesheet_fit_data.tgz -o data/module_specific_input_data/fittedismip-gris/FittedISMIP_icesheet_fit_data.tgz
tar -xzf data/module_specific_input_data/fittedismip-gris/FittedISMIP_icesheet_fit_data.tgz -C data/module_specific_input_data/fittedismip-gris
```

### bamber19-icesheets

Download size: 236 MB

```bash
mkdir -p data/module_specific_input_data/bamber19-icesheets
curl -L https://zenodo.org/record/7478192/files/bamber19_icesheets_preprocess_data.tgz -o data/module_specific_input_data/bamber19-icesheets/bamber19_icesheets_preprocess_data.tgz
tar -xzf data/module_specific_input_data/bamber19-icesheets/bamber19_icesheets_preprocess_data.tgz -C data/module_specific_input_data/bamber19-icesheets
```

### deconto21-ais

Download size: 765 KB

```bash
mkdir -p data/module_specific_input_data/deconto21-ais
curl -L https://zenodo.org/record/7478192/files/deconto21_AIS_preprocess_data.tgz -o data/module_specific_input_data/deconto21-ais/deconto21_AIS_preprocess_data.tgz
tar -xzf data/module_specific_input_data/deconto21-ais/deconto21_AIS_preprocess_data.tgz -C data/module_specific_input_data/deconto21-ais
```

### ipccar5-glaciers

Download size: 23 KB

```bash
mkdir -p data/module_specific_input_data/ipccar5
curl -L https://zenodo.org/record/7478192/files/ipccar5_glaciers_project_data.tgz -o data/module_specific_input_data/ipccar5/ipccar5_glaciers_project_data.tgz
tar -xzf data/module_specific_input_data/ipccar5/ipccar5_glaciers_project_data.tgz -C data/module_specific_input_data/ipccar5
```

### ipccar5-icesheets

Download size: 4 KB

```bash
mkdir -p data/module_specific_input_data/ipccar5
curl -L https://zenodo.org/record/7478192/files/ipccar5_icesheets_project_data.tgz -o data/module_specific_input_data/ipccar5/ipccar5_icesheets_project_data.tgz
tar -xzf data/module_specific_input_data/ipccar5/ipccar5_icesheets_project_data.tgz -C data/module_specific_input_data/ipccar5
```

### larmip-ais

Download size: 131 KB (two downloads)

```bash
mkdir -p data/module_specific_input_data/larmip-ais
curl -L https://zenodo.org/record/7478192/files/larmip_icesheet_fit_data.tgz -o data/module_specific_input_data/larmip-ais/larmip_icesheet_fit_data.tgz
tar -xzf data/module_specific_input_data/larmip-ais/larmip_icesheet_fit_data.tgz -C data/module_specific_input_data/larmip-ais
curl -L https://zenodo.org/record/7478192/files/larmip_icesheet_project_data.tgz -o data/module_specific_input_data/larmip-ais/larmip_icesheet_project_data.tgz
tar -xzf data/module_specific_input_data/larmip-ais/larmip_icesheet_project_data.tgz -C data/module_specific_input_data/larmip-ais
```

### ssp-landwaterstorage

Download size: 224 KB (two downloads)

```bash
mkdir -p data/module_specific_input_data/ssp-landwaterstorage
curl -L https://zenodo.org/record/7478192/files/ssp_landwaterstorage_preprocess_data.tgz -o data/module_specific_input_data/ssp-landwaterstorage/ssp_landwaterstorage_preprocess_data.tgz
tar -xzf data/module_specific_input_data/ssp-landwaterstorage/ssp_landwaterstorage_preprocess_data.tgz -C data/module_specific_input_data/ssp-landwaterstorage

curl -sL https://zenodo.org/record/7478192/files/ssp_landwaterstorage_postprocess_data.tgz -o data/module_specific_input_data/ssp-landwaterstorage/ssp_landwaterstorage_postprocess_data.tgz
tar -xzf data/module_specific_input_data/ssp-landwaterstorage/ssp_landwaterstorage_postprocess_data.tgz -C data/module_specific_input_data/ssp-landwaterstorage
```

### tlm-sterodynamics

Download size: **31.1 GB** (two downloads: 2 KB + 31.1 GB)

!!! Warning
    The CMIP6 data for this module is very large (31.1 GB compressed). Make sure you have enough disk space (the extracted data needs additional space on top of the download) and a reliable connection before starting.

```bash
mkdir -p data/module_specific_input_data/tlm-sterodynamics
curl -L https://zenodo.org/record/7478192/files/tlm_sterodynamics_preprocess_data.tgz -o data/module_specific_input_data/tlm-sterodynamics/tlm_sterodynamics_preprocess_data.tgz
tar -xzf data/module_specific_input_data/tlm-sterodynamics/tlm_sterodynamics_preprocess_data.tgz -C data/module_specific_input_data/tlm-sterodynamics

curl -L https://zenodo.org/record/7478192/files/tlm_sterodynamics_cmip6_data.tgz -o data/module_specific_input_data/tlm-sterodynamics/tlm_sterodynamics_cmip6_data.tgz
tar -xzf data/module_specific_input_data/tlm-sterodynamics/tlm_sterodynamics_cmip6_data.tgz -C data/module_specific_input_data/tlm-sterodynamics
```

### ebm3-sterodynamics

Download size: **31.3 GB** (two downloads: 1.9 MB + 31.3 GB)

!!! Warning
    The CMIP6 data for this module is very large (31.3 GB compressed). Make sure you have enough disk space (the extracted data needs additional space on top of the download) and a reliable connection before starting.

```bash
mkdir -p data/module_specific_input_data/ebm3-sterodynamics/ebm3_project_data
curl -L https://zenodo.org/records/16024082/files/ebm3_project_data.tgz -o data/module_specific_input_data/ebm3-sterodynamics/ebm3_project_data.tgz
tar -xzf data/module_specific_input_data/ebm3-sterodynamics/ebm3_project_data.tgz -C data/module_specific_input_data/ebm3-sterodynamics/ebm3_project_data

curl -L https://zenodo.org/records/16024082/files/tlm_sterodynamics_cmip6_ebm3_data.tgz -o data/module_specific_input_data/ebm3-sterodynamics/tlm_sterodynamics_cmip6_ebm3_data.tgz
tar -xzf data/module_specific_input_data/ebm3-sterodynamics/tlm_sterodynamics_cmip6_ebm3_data.tgz -C data/module_specific_input_data/ebm3-sterodynamics
```

### kopp14-verticallandmotion

Download size: 653 KB

```bash
mkdir -p data/module_specific_input_data/kopp14-verticallandmotion
curl -L https://zenodo.org/record/7478192/files/kopp14_verticallandmotion_preprocess_data.tgz -o data/module_specific_input_data/kopp14-verticallandmotion/kopp14_verticallandmotion_preprocess_data.tgz
tar -xzf data/module_specific_input_data/kopp14-verticallandmotion/kopp14_verticallandmotion_preprocess_data.tgz -C data/module_specific_input_data/kopp14-verticallandmotion
```

### oelsmann24-verticallandmotion

Download size: 1.3 GB

```bash
mkdir -p data/module_specific_input_data/oelsmann24-verticallandmotion
curl -L https://zenodo.org/records/18199757/files/oelsmann24_vlm_data.tar.gz -o data/module_specific_input_data/oelsmann24-verticallandmotion/oelsmann24_vlm_data.tar.gz
tar -xzf data/module_specific_input_data/oelsmann24-verticallandmotion/oelsmann24_vlm_data.tar.gz -C data/module_specific_input_data/oelsmann24-verticallandmotion
mv data/module_specific_input_data/oelsmann24-verticallandmotion/data/* data/module_specific_input_data/oelsmann24-verticallandmotion/ && rmdir data/module_specific_input_data/oelsmann24-verticallandmotion/data
```

### nzinsargps-verticallandmotion

Download size: 144 KB

```bash
mkdir -p data/module_specific_input_data/nzinsargps-verticallandmotion
curl -L https://zenodo.org/record/7478192/files/NZInsarGPS_verticallandmotion_preprocess_data.tgz -o data/module_specific_input_data/nzinsargps-verticallandmotion/NZInsarGPS_verticallandmotion_preprocess_data.tgz
tar -xzf data/module_specific_input_data/nzinsargps-verticallandmotion/NZInsarGPS_verticallandmotion_preprocess_data.tgz -C data/module_specific_input_data/nzinsargps-verticallandmotion
```