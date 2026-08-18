# About the FACTS-module-registry entry (module YAML file)
Here, you will find a description of the different sections of a module YAML file and the information required in each. 

## Skeleton module YAML file
```yaml
# MY MODULE CONFIGURATION
# Input data archive:
# https://zenodo.org/records/<record-id>/files/<archive>.tgz

module_name: "my-module"
container_image: "ghcr.io/fact-sealevel/my-module:0.1.0"
command: "main"
uses_climate_file: false
climate_file_required: false

arguments:
  top_level:          [...]   # experiment-wide values, from metadata.*
  options:            [...]   # module-specific knobs, from module_inputs.options.*
  fingerprint_params: [...]   # sea-level fingerprint dirs/files
  inputs:             [...]   # files the module reads
  outputs:
    files:            [...]   # files the module writes
    other:            [...]   # non-file outputs (directories)

volumes:              {...}   # named host→container mounts
```
---
## Top-level keys
| Key | Type | Required | Notes |
|---|---|---|---|
| `module_name` | str | yes | Kebab-case; must equal the directory name |
| `container_image` | str | yes | Full image URI with tag or digest |
| `arguments` | map | yes | See §6 |
| `volumes` | map | yes | See §7 |
| `command` | str | usually | Subcommand passed before the flags. Use `"main"` for single-command CLIs |
| `uses_climate_file` | bool | usually | `true` if the module consumes climate-step output |
| `climate_file_required` | bool | usually | `true` if that climate file is mandatory |
| `depends_on` | list | no | Compose service dependencies |
| `input_dir_name` | str | no | See below |
| `skip_fingerprint_params` | bool | no | `true` if the module has no fingerprint arguments |
| `per_workflow` | bool | no | `true` instantiates one service per workflow rather than per module (used by `facts-total`) |
| `output_types` | list | no | Defaults to `[global, local]` |
 
**Pinning the image.** Either a semver tag (`my-module:0.1.0`) or a digest
(`my-module@sha256:281e89...`, as `emulandice2-*` does). A digest is exactly reproducible;
a tag is easier to read. Do not use `:latest`.
 
**`command` and multi-command images.** If your container's entrypoint is a Click *group*
with subcommands, each subcommand gets its own registry entry pointing at the same image.
`ipccar5` does this: `ipccar5-glaciers` sets `command: "glaciers"` and `ipccar5-icesheets`
sets `command: "icesheets"`, both on `ghcr.io/fact-sealevel/ipccar5:0.1.2`.
 
**`input_dir_name`.** By default FEB expects a module's input data in a subdirectory named
after the module (`module_specific_input_data/my-module/`). Sibling entries sharing one
image usually share one input directory too, so both `ipccar5-*` entries set
`input_dir_name: "ipccar5"`, and all three `emulandice2-*` entries set
`input_dir_name: "emulandice2"`.
 
---

## The `arguments` section
 
Five subsections. The split matters because it determines **where FEB looks for the
value** — that is the `source` dot-path — and which fields are legal.
 
Every entry, in every subsection, needs at least:
 
```yaml
- name: "chunksize"                            # becomes --chunksize
  type: "int"                                  # str | int | float | bool | file
  source: "module_inputs.options.chunksize"    # where FEB reads the value
```
 
`name` must match the CLI flag exactly, minus the leading `--`. FEB emits
`--{name}={value}`.

### Which fields each subsection allows
 
| Field | `top_level` | `options` | `fingerprint_params` | `inputs` | `outputs.files` | `outputs.other` |
|---|:-:|:-:|:-:|:-:|:-:|:-:|
| `name`, `type`, `source` | ✅ req | ✅ req | ✅ req | ✅ req | ✅ req | ✅ req |
| `help` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `optional` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `mount` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `alternatives` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `transform` | ✅ | — | ✅ | — | — | — |
| `default_value` | — | ✅ | ✅ | ✅ | — | — |
| `filename` | — | — | ✅ | ✅ (str or list) | ✅ | — |
| `filename_map` | — | — | — | ✅ | ✅ | — |
| `multiple` | — | ✅ | — | ✅ | — | — |
| `envvar` | — | ✅ | — | ✅ | — | — |
| `allowed_values` | — | ✅ | — | — | — | — |
| `external_volume` | — | — | — | ✅ | — | — |
| `climate_step_output` | — | — | — | ✅ | — | — |
| `output_type` | — | — | — | — | ✅ **req** | — |
| `pass_to_total` | — | — | — | — | ✅ (default `true`) | — |
 
Note `optional: true` means *FEB will not error if no value is supplied* — it is about
FEB's config handling, not about whether your CLI marks the flag `required`.
 
---

### Arguments: `top_level`
 
Values that apply to the whole experiment, sourced from `metadata.*`. FEB collects these
across all selected modules and prompts the user for them once.
 
```yaml
top_level:
  - name: "pipeline-id"
    type: "str"
    source: "metadata.pipeline-id"
    help: "Unique identifier for the pipeline running this code"
  - name: "nsamps"
    type: "int"
    source: "metadata.nsamps"
    help: "Number of samples to generate"
  - name: "baseyear"
    type: "int"
    source: "metadata.baseyear"
    help: "Year from which projections are zeroed"
  - name: "pyear-start"
    type: "int"
    source: "metadata.pyear_start"
  - name: "pyear-end"
    type: "int"
    source: "metadata.pyear_end"
  - name: "pyear-step"
    type: "int"
    source: "metadata.pyear_step"
  - name: "location-file"
    type: "str"
    source: "metadata.location-file"
    optional: true
    transform: "filename"
    help: "File containing name, id, lat, and lon of points for localization"
    mount:
      volume: "shared_input"
      container_path: "/mnt/shared_in"
```
 
Conventional sources — reuse these spellings so values are shared rather than duplicated:
 
| Source | Meaning |
|---|---|
| `metadata.pipeline-id` | Run identifier |
| `metadata.scenario` | Emissions scenario |
| `metadata.nsamps` | Sample count |
| `metadata.baseyear` | Reference/zeroing year |
| `metadata.pyear_start` / `_end` / `_step` | Projection year range |
| `metadata.location-file` | Localization site list |
 
Your CLI's flag name does **not** have to match the source. `ipccar5-glaciers` maps
`metadata.baseyear` onto its `--start-year` flag:
 
```yaml
  - name: "start-year"
    type: "int"
    source: "metadata.baseyear"
    optional: true
    help: "Year from which to start integrating temperature"
```
 
### Arguments: `options`
 
Module-specific settings, sourced from `module_inputs.options.*`. These become the tunable
block in the user's `experiment-config.yaml`. Give every option a `help` string and a
`default_value` where one is sensible — this is the main documentation users see.
 
```yaml
options:
  - name: "chunksize"
    type: "int"
    source: "module_inputs.options.chunksize"
    optional: true
    default_value: 1000
    help: "Number of locations to process in each chunk"
  - name: "rng-seed"
    type: "int"
    source: "module_inputs.options.seed"
    optional: false
    default_value: 1234
    help: "Seed for the random number generator."
  - name: "region"
    type: "str"
    source: "module_inputs.options.region"
    default_value: "ALL"
    allowed_values: ["ALL", "EAIS", "WAIS", "PEN"]
    help: "Region within this ice source to use."
```
 
Note the convention that a `--rng-seed` flag reads from `module_inputs.options.seed`, so
seeds are consistent across modules.
 
### Arguments: `fingerprint_params`
 
Sea-level fingerprint files and directories that translate global to local sea level,
sourced from `module_inputs.fingerprint_params.*`. These live in the **shared** input
directory, since several modules use the same fingerprint data.
 
```yaml
fingerprint_params:
  - name: "fingerprint-dir"
    type: "str"
    source: "module_inputs.fingerprint_params.fingerprint_dir"
    default_value: FPRINT
    help: "Directory containing sea-level fingerprint files"
    mount:
      volume: "shared_input"
      container_path: "/mnt/shared_in"
```
 
If your module needs none, omit the subsection **and** set `skip_fingerprint_params: true`
at the top level.
 
### Arguments: `inputs`
 
Files the module reads, sourced from `module_inputs.inputs.*`. Declare `filename` for each
one — that string is what `check-data` looks for on disk, so it is how users learn what to
download.
 
```yaml
inputs:
  - name: "gia-stats-input-file"
    type: "file"
    source: "module_inputs.inputs.gia_stats_input_fname"
    filename: "GIA_stats.nc"
    help: "Path to GIA statistics netCDF file"
    mount:
      volume: "module_specific_input"
      container_path: "/mnt/module_specific_in"
```
 
**Consuming climate output (cross-module input).** If your module reads a file produced by
the climate step, set `external_volume: true`, mount from the `output` volume, and name the
argument `climate-data-file` (or `input-data-file`). For those two names FEB *requires*
`climate_step_output`, which names the output argument in the climate module's YAML that
you want:
 
```yaml
  - name: "climate-data-file"
    type: "file"
    source: "module_inputs.inputs.climate_data_file"
    help: "NetCDF file containing surface temperature data"
    climate_step_output: "output-climate-file"   # or "output-gsat-file"
    external_volume: true
    mount:
      volume: "output"
      container_path: "/mnt/out"
      transform: "filename"
```
 
Valid `climate_step_output` values are output `name`s declared by the climate modules —
`fair-temperature` and `fair2-climate` both expose `output-climate-file`,
`output-gsat-file`, `output-oceantemp-file`, and `output-ohc-file`. Pair this with
`uses_climate_file: true` (and `climate_file_required` as appropriate) at the top level.
 
**Repeatable flags.** `multiple: true` lets FEB emit the flag once per value:
 
```yaml
  - name: "item"
    type: "file"
    source: "module_inputs.inputs.item"
    multiple: true
    mount:
      volume: "input"
      container_path: "/mnt/total_in"
```
 
**Env-var inputs.** Some values must reach the container as environment variables rather
than flags (the `emulandice` R harness needs this). Use `envvar`:
 
```yaml
  - name: "forcing-head-path"
    type: "file"
    source: "module_inputs.inputs.forcing_head_path"
    envvar: "EMULANDICE_FORCING_HEAD_PATH"
```
 
### Arguments: `outputs`
 
Split into `files` (things with a filename and an output type) and `other` (directories and
other non-file outputs). Sourced from `module_inputs.outputs.*`.
 
```yaml
outputs:
  files:
    - name: "output-lslr-file"
      type: "file"
      source: "module_inputs.outputs.output_lslr_file"
      filename: "lslr.nc"
      output_type: "local"
      pass_to_total: true
      help: "Output local sea-level rise projections file"
      mount:
        volume: "output"
        container_path: "/mnt/out"
        transform: "filename"
    - name: "output-quantiles-file"
      type: "file"
      source: "module_inputs.outputs.output_quantiles_file"
      filename: "quantiles.nc"
      output_type: "local"
      pass_to_total: false
      help: "Output quantiles file"
      mount:
        volume: "output"
        container_path: "/mnt/out"
        transform: "filename"
  other:
    - name: "output-glacier-dir"
      type: "str"
      source: "module_inputs.outputs.output_glacier_dir"
      optional: true
      help: "Directory into which per-glacier GSLR files are written"
      mount:
        volume: "output"
        container_path: "/mnt/out"
```
 
`output_type` is **required** on every `files` entry:
 
| Value | Meaning |
|---|---|
| `global` | Global mean sea-level contribution |
| `local` | Localized sea level at the requested sites |
| `total` | Aggregated total across components |
| `esl` | Extreme sea level |
 
`pass_to_total` (default `true`) controls whether the file is fed into `facts-total`.
Set it to `false` for diagnostics like quantile files, which would otherwise be
double-counted.
 
Output mounts almost always carry `transform: "filename"`, so the CLI receives a bare
filename and the container writes it into the mounted output directory.
 
---
 
## The `volumes` section
 
Declares the named bind mounts your arguments reference. **Every `mount.volume` value used
anywhere in `arguments` must appear as a key here.**
 
```yaml
volumes:
  module_specific_input:
    host_path: "module_inputs.input_paths.module_specific_input_dir"
    container_path: "/mnt/module_specific_in"
    help: "Input data specific to this module."
  shared_input:
    host_path: "module_inputs.input_paths.shared_input_dir"
    container_path: "/mnt/shared_in"
    help: "Input data shared across modules (fingerprints, location file)."
  output:
    host_path: "module_inputs.output_paths.output_dir"
    container_path: "/mnt/out"
    help: "Directory for this module's output files."
```
 
| Field | Notes |
|---|---|
| `host_path` | Dot-path FEB resolves to a real host directory |
| `container_path` | Absolute mount point inside the container |
| `help` | Free text |
 
Use those three volumes and those three `host_path` values unless you have a specific
reason not to. Two conventions are load-bearing:
 
- **Name the output volume exactly `output`.** FEB special-cases this key when building
  mounts and when deciding how to rewrite output paths.
- **The output volume's `host_path` must contain `output_paths`.** FEB identifies the
  output volume by searching for that substring, which is how it detects cross-module
  inputs.
`container_path` is declared in two places — on the volume and on each `mount`. Keep them
consistent; a mismatch is the most common cause of a module that starts but cannot find
its files.
 
---

## Transforms
 
`transform` applies a named function to a value before it reaches the CLI. Only three names
are implemented in FEB today:
 
| Transform | Effect |
|---|---|
| `filename` | Passes only the basename, not the full host path |
| `scenario_name` | Extracts the scenario string from FEB's scenario object |
| `scenario_name_ssp_landwaterstorage` | Applies the `ssp-landwaterstorage` scenario mapping |
 
Adding a new module-specific mapping is **not** purely a registry change: FEB dispatches on
the literal transform name in `module_service_spec.py`, so a new
`scenario_name_<module>` transform needs a matching branch there plus a function in
`core/transforms.py`. Open an issue if you need one, and the maintainers can help.
 
The mapping file itself is a flat key-value YAML:
 
```yaml
# scenario_name_mapping_my_module.yaml
ssp585: "SSP5-8.5"
ssp370: "SSP3-7.0"
ssp245: "SSP2-4.5"
ssp126: "SSP1-2.6"
```