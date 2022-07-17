# Changelog


## v20220717 | 17 Jul 2022
- Added new setting `data_vars_parse_pos_indices` to filegroups settings. If `true`, the
script tries to parse the horizontal, vertical and replicate indices from the variable
name. This is mainly necessary for meteo data, which have these indices as part of their
filename, e.g. `SW_IN_T1_35_1`, where `T1` is the horizontal index, `35` the vertical
index (35m), and `1` the replicate index. These indices are used as tags in the database.
Flux data normally do not have these indices, there e.g. the CO2 flux is simply called `FC`.
For such data that do not have these indices in the variable names, the flag is set to `false`.
This is currently the case for `PROC-EDDYPRO-FLUXNET-CSV-L0-30MIN` and `PROC-FLUXNET-FULLSET-HH-CSV-30MIN`.
- Tried to harmonize `measurement` groups for EddyPro FLUXNET and FLUXNET FULLSET output
