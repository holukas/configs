# Changelog

## v20220922 | 22 Sep 2022

### Changes to CH-LAE filetype

- Adjusted variables in `LAE11-RAW-TBL1-202205311130-TOA5-DAT-30MIN.yaml`

## v20220921 | 21 Sep 2022

### Additional changes to CH-AWS filetypes

- Adjusted variable naming in `AWS10-RAW-TBL1-201701260236-TOA5-DAT-1MIN.yaml`
- Adjusted variable naming in `AWS11-RAW-TBL1-201909190000-TOA5-DAT-30MIN.yaml`
- Adjusted variable naming in `AWS15-RAW-201911130000-TOA5-DAT-1MIN.yaml`

## v20220909 | 9 Sep 2022

### Changes to CH-AWS filetypes

- Renamed vars in `AWS10-RAW-TBL1-201701260236-TOA5-DAT-1MIN.yaml`, there is no M1
  here, only `T1`
- Added and adjusted vars in `AWS11-RAW-TBL1-201909190000-TOA5-DAT-30MIN.yaml`, meteo
  data in this file are all `M3`
- Adjusted varnames in `AWS15-RAW-201911130000-TOA5-DAT-1MIN.yaml`, meteo data in
  this file are all `M1`, the correct grassland floor is `GF2`

## v20220816 and v20220817 | 17 Aug 2022

- Updated PA info in `DAV10-RAW-PA-202112020000-TOA5-DAT-10S` and
  `DAV17-RAW-201807110000-ICOS-PRF-DAT-10S`.

## v20220729 | 29 Jul 2022

- `OE210-RAW-TBL1-201703150939-TOA5-DAT-1MIN`: Added additional radiation variables
  from parallel measurements that started in June 2022.
- `DAV11-RAW-TBL2-201804051700-TOA5-DAT-1H`: Changed `units` and `gain` for SWC variables
  to `units: "%", gain: 100`.

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
