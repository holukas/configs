# Changelog

## v20230128 | 28 Jan 2023

- Renamed `DAV11-RAW-TBL3-201812060001-TOA5-DAT-1H` to `DAV11-RAW-TBL3-201812060001-TOA5-DAT-1MIN`.
  Configuration for this file was not correct because the data are indeed in 1MIN time resolution,
  and not in 1H. File was adjusted accordingly.
    - The precipitation in this files is the main precipitation for this site. Accordingly, it was
      renamed to `PREC_TOT_T1_20_1`. (in `DAV11-RAW-TBL3-201812060001-TOA5-DAT-1MIN`)
    - Precipitation was renamed to `PREC_TOT_LOWRES_T1_20_1` (in `DAV11-RAW-TBL1+-201108250000-TOA5-DAT-10MIN`)

### Working on `FRU` filetypes

- Added filetype `FRU10-RAW-201509160815-A-30MIN`

## v20221205 | 5 Dec 2022

### Working on `LAE` filetypes

More `LAE` filetypes needed due to changing time resolutions in files.

- Added filetype `LAE11-RAW-TBL1-201912191250-TOA5-DAT-10MIN`
- Added filetype `LAE11-RAW-TBL1-202012010000-TOA5-DAT-1MIN`
- Changed horizontal position index for `RH_T1_2_1_Avg` and `TA_T1_2_1_Avg`: both of them are mislabelled
  in the raw data files and are correctly at hpos `M1`.

### Working on `DAV` filetypes

- Renamed filetype `DAV10-RAW-TBL1-201802281101-TOA5-DAT-1MIN`. Was wrongly named with `10S` before.

## v20221126 | 26 Nov 2022

### Working on `DAV10` filetypes

- Added filetype `DAV10-RAW-PROFILE-201308010000-NOFILEDATE-TOA5-DAT-10MIN`
- Added filetype `DAV10-RAW-PROFILE-200511081609-A-10MIN`
- Added filetype `DAV10-RAW-PROFILE-200606131327-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-200612201820-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-200704171410-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-200806271118-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-200811211210-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-200905131053-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-201009252127-ALTERNATING-A-10MIN` (SPECIAL FORMAT `-ALTERNATING-`)
- Added filetype `DAV10-RAW-PROFILE-201108251403-TOA5-DAT-10MIN`
- Added filetype `DAV10-RAW-RAD-201003091309-A-10MIN`
- Added filetype `DAV10-RAW-RAD-200606131632-A-30MIN`
- Added filetype `DAV10-RAW-RAD-200612200949-A-30MIN`
- Added filetype `DAV10-RAW-RAD-200704171403-A-1MIN`
- Added filetype `DAV10-RAW-RAD-201407161745-CSV-GZ-1MIN`
- Added filetype `DAV10-RAW-TBL1-201501010645-CSV-GZ-1MIN`
- Added filetype `DAV10-RAW-TBL1-201807181036-TOA5-DAT-10S`
- Adjusted resolution and end for filetype `DAV10-RAW-TBL1-201802281101-TOA5-DAT-1MIN`

### Working on `LAE` filetypes

- Harmonized variable naming among filetypes
- Added filetype `LAE12-RAW-TBL1-201610272240-TOA5-DAT-10MIN`
- Added filetype `LAE11-RAW-TBL1-201605301110-TOA5-DAT-1MIN`
- Added filetype `LAE10-RAW-PROFILE-202205100000-TOA5-DAT-1MIN`

## v20221104 | 4 Nov 2022

- Added filetype `OE210-RAW-TBL2-202002112120-TOA5-DAT-10MIN`

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
