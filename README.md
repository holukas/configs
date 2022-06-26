# configs

This folder contains configuration files needed to run `POET` scripts.
Currently used by the packages [dataflow](https://gitlab.ethz.ch/poet/dataflow) and [dbc](https://gitlab.ethz.ch/poet/dbc).

Note that the database configuration is not stored in `configs`, but in a separate folder 
that has the same name as the `configs` folder but with the suffix `_secret`. If the `configs`
folder is given e.g. in the script `dataflow` as `C:\configs`, then the database configuration
is assumed to be stored in folder `C:\configs_secret`.
