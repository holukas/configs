# Changelog

## 20240905 | 5 Sep 2024

- Added new variables to `TAN10-RAW-BOX1-202311281911-TOA5-DAT-1MIN.yaml`

## 20240829 | 29 Aug 2024

- Changed `vol%` to `%` as given units for SWC in `OE210-RAW-TBL2-202002112120-TOA5-DAT-10MIN`

## 20240828 | 28 Aug 2024

- Added lag variables for `N2O` in filetype `PROC-EDDYPRO-FLUXNET-CSV-L0-30MIN`
- Units `vol%` was changed to `%`

## 20240811 | 11 Aug 2024

- `PROC-MS-MAIER2022-HH-CSV-30MIN` for [Maier et al. (2022)](https://doi.org/10.1016/j.scitotenv.2022.157541)
- `PROC-MS-KREBS2024-CMB-CSV-D` for [Krebs et al. (2024)](https://doi.org/10.5194/bg-21-2005-2024)
- `PROC-MS-KREBS2024-CMB-CSV-30MIN` for [Krebs et al. (2024)](https://doi.org/10.5194/bg-21-2005-2024)

## 20240708 | 8 Jul 2024

- Added new setting `parse_pos_indices` to parse position indices from specific variables where this information is
  available from the variable name (field). If set to 'true' (not capitalized because YAML), this overrides the setting
  `data_vars_parse_pos_indices` at the file-level. One example filetype where this is applied is the
  filetype `PROC-MS-FEIGENWINTER2023B-HH-CSV-30MIN`, in which position indices are available for soil measurements, but
  not for other variables. In this case, the corresponding settings are `data_vars_parse_pos_indices: false` at the
  file-level, and `parse_pos_indices: true` in the settings for the specific variables. Nice!
- Added new filetype `PROC-MS-FEIGENWINTER2023A-HH-CSV-30MIN` for data used in the manuscript
  by [Feigenwinter et. al (2023a)](https://doi.org/10.1016/j.agrformet.2023.109613)
- Added new filetype `PROC-MS-FEIGENWINTER2023B-HH-CSV-30MIN` for data used in the manuscript
  by [Feigenwinter et. al (2023b)](https://doi.org/10.1016/j.scitotenv.2023.166389)
- Added new filetype `PROC-ICOS-FLUXNET-L2-HH-CSV-30MIN` for the ICOS L2 data product
- Added new filetype `PROC-MS-KREBS2024-CMB-CSV-30MIN` for data used in the
  manuscript [Krebs et al. (2024)](https://doi.org/10.5194/bg-21-2005-2024)
- Updated filetype `HON10-RAW-202402261031-TOA5-DAT-1MIN`

## 20240704 | 4 Jul 2024

- Added new filetype `PROC-ETH-METEOSCREENINGTOOL-CSV-30MIN` for meteo data screened using the MeteoScreeningTool
- Added new filetype `PROC-FLUXNET-WW2020-FULLSET-HH-CSV-30MIN` for
  the [WW2020 FLUXNET/ICOS data product](https://www.icos-cp.eu/data-products/2G60-ZHAK)
- Added new setting `offset` to filetype configs. If not specifically given, `offset=0.0` (float) is assumed
- Added new setting `ignore_between` to filetype configs to ignore data between two dates
- Renamed and adjusted time range in filetype `CHA10-RAW-TBL0-201603171458-CSV-GZ-1MIN`
- Fixed TS depths in `CHA10-RAW-TBL0-201603171458-CSV-GZ-1MIN`
- Added new filetype `CHA10-RAW-TBL0-201606210945-CSV-GZ-1MIN` to calculate LW variables from raw variables during a
  short time period where the original measurements are not available
- Added new filetype `CHA10-RAW-TBL0-201606291845-CSV-GZ-1MIN`
- Fixed TS depths in `CHA10-RAW-TBL1-201701010000-TOA5-DAT-1MIN`
- Added units for `TM` measurements in `CHA10-RAW` files
- Adding corrections (gain, rawfuncs) from the previous Python MeteoScreeningTool for `CH-CHA`
- All `WS` and `WD` both are now measurement `WIND`
- Changed type of `gain` from integer to float, e.g. `gain: 100` is now `gain: 100.0` to be consistent. Likewise, if
  no `gain` is provided, then `gain: 1.0` (float) is assumed
- Removed all `gain: 1` settings from all filetype configs, because it is assumed `1.0` if not specifically given
- Updated README
- Folder for filetypes processed is now named `processed`
- Removed: filetype setting `data_keep_date_col`
- Renamed settings to `data_timestamp_column`, `data_timestamp_timezone_offset_to_utc_hours` and `data_timestamp_format`
- Removed setting `data_date_parser`
- Changed: Level-0 fluxes from EddyPro now have the setting `data_version: eddypro_level-0`
  or `data_version: eddypro_level-0`

## v20240612 | 12 Jun 2024

- Added new filetype `FOR10-RAW-202403141026-TOA5-DAT-1MIN`
- Added new filetype `HON10-RAW-202402261031-TOA5-DAT-1MIN`
- Changed some `measurement` settings

## v20240611 | 11 Jun 2024

- Updated settings for `CHA10` files to avoid data overlaps, renamed `CHA10-RAW-TBL1-201605230000-TOA5-DAT-1MIN.yaml`
  to `CHA10-RAW-TBL1-201701010000-TOA5-DAT-1MIN`
- Added updated raw variable names in `CHA10-RAW-TBL1-201701010000-TOA5-DAT-1MIN`
- Added units in `CHA10-RAW-TBL0-20160317145800-CSV-GZ-1MIN`

## v20240527 | 27 May 2024

- Added new site `CH-FOR`
- Added new site `CH-HON`

## v20240507 | 7 May 2024

### Multiple IDs to identify data rows

It is now possible to define multiple IDs that identify good data rows. This is done by specifying
e.g. `data_keep_good_rows: [ 0, [ 102, 103 ], [ 202, 203 ] ]`, which means that all data rows that start with
either `102` or `103` are kept and use variable info in `data_vars`, and `202` or `203` use the variable info given
in `data_vars2`.
Up to this update always single integers were given instead of a list, which is also still possible like before. In
this case all records that start with that integer are kept. For example, `data_keep_good_rows: [ 0, 102, 202 ]`,
which means that all data rows that start with `102` are kept and use variable info in `data_vars`, and all data rows
that start with `202` use the variable info given in `data_vars2`.

### Changes

- Udpated all frequency settings in `data_raw_freq` to new `pandas` DateOffsets (
  see [here](https://pandas.pydata.org/docs/user_guide/timeseries.html#dateoffset-objects)). This means that e.g. a file
  with 30-minute time resolution now has the setting `30min` (previously it was `30T`).
- Added `99999` and `-99999` as pre-defined code for missing values in all settings
  files: `data_na_values: [ -9999, nan, NaN, NAN, NA, -6999, '-' , 99999, -99999]`

### New filetypes

- Added new filetype: `CHA10-RAW-LOGGER-200507151448-ALTERNATING-ID-A-30MIN`

## v20240405 | 5 Apr 2024

- Updated Basedirs network addresses from Windows (network locations) in `dirs.yaml`

## v20240322 | 22 Mar 2024

- Fixed TS name in `TAN10-RAW-BOX1-202311281911-TOA5-DAT-1MIN`

## v20240313 | 13 Mar 2024

- Added new site `ch-tan: CH-TAN_Taenikon` in `dirs.yaml`
- Added new filetype `TAN10-RAW-BOX1-202311281911-TOA5-DAT-1MIN`

## v20240129 | 29 Jan 2024

- Fixed wrong `filetype_valid_from` in `FRU10-RAW-TBL1-201811051615-TOA5-DAT-1MIN`

## v20240102 | 2 Jan 2024

- Added new setting `data_special_format`, added info for `-ALTERNATING-` and `-ICOSSEQ-` filetypes, all
  other filetypes are set to `false`.
- Added new setting `data_vars_order` in filetype configs, which defines whether the variables listed in
  `data_vars` are listed in the exact sequence they appear in the file (`strict`), or if they are
  listed freely (`free`) and their sequence does not matter. The `strict` setting is important for
  old filetypes that do not have variable names in their headers.
- Fixed typo bug in settings
- Renamed `DAV11-RAW-TBL1-201108250000-TOA5-DAT-10MIN`
- Added LEAF_WET variables to `DAV12-RAW-FF6-TBL1-202107292014-TOA5-DAT-1MIN`
- Added LEAF_WET variables to `LAE12-RAW-TBL1-202103291559-TOA5-DAT-1MIN`
- Much more info added to `README`

## v20231208 | 8 Dec 2023

### Early logger meteo files FRU10

- Added `FRU10-RAW-LOGGER-2005071513-A-30MIN`: this is one of the earliest meteo files. These
  early files do not have variable names in the header. Typically, there is a so-called `config.header`
  file that gives info about variable names and column order. However, in this case the number of
  variables given in the `config.header` file did not match the data files, there were two columns too
  many.
  I found info about variable names and column order in the file `CH-FRU_CONFIG_Linux.xlsx`, which
  is the config file for the MeteoScreening Python script that was used previously. The info given
  in this file shows two columns less than the `config.header` file and therefore matches the data
  files.
  The two variables that are listed in `CH-FRU_CONFIG_Linux.xlsx` but not in `config.header` are
  `LiBatt_V_AVG` and `LiBatt_V_STD`.
- Added new setting in config files: it is now possible to assign a data function `rawfunc` to
  specific variables,
  e.g., `Theta_11_STD: { field: SDP_SD_GF1_0.05_1, units: mV, gain: 1, rawfunc: calc_swc, measurement: _SD }`.
  In this example a function that calculates SWC (soil water content) from SDP is applied. This setting
  is currently picked up and applied by the Python script `dataflow`.
- Two time resolutions can now be given for `-ALTERNATING-` formats, one for each ID: `data_raw_freq: [ 30min, 10min ]`.
  It is also possible to define an irregular timestamp, e.g., `[ 30min, irregular ]`
- Added and updated many FRU filetypes

### Forest floor TBL1 and TBL2 files DAV12

- Added new filetype `DAV12-RAW-FF3-TBLSAP-202106071716-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF1-TBL1-201410231208-CSV-GZ-30MIN`
- Added new filetype `DAV12-RAW-FF1-TBL1-201411121850-CSV-GZ-10MIN`
- Added new filetype `DAV12-RAW-FF1-TBL2-201507021347-CSV-GZ-25H`

- Added new filetype `DAV12-RAW-FF2-TBL1-201410231208-CSV-GZ-30MIN`
- Added new filetype `DAV12-RAW-FF2-TBL1-201412041850-CSV-GZ-10MIN`
- Added new filetype `DAV12-RAW-FF2-TBL1-201802281110-TOA5-DAT-10MIN`
- Added new filetype `DAV12-RAW-FF2-TBL1-201807181510-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF2-TBL2-201507021850-CSV-GZ-25H`
- Added new filetype `DAV12-RAW-FF2-TBL2-201802281304-TOA5-DAT-25H`

- Added new filetype `DAV12-RAW-FF3-TBL1-201410231208-CSV-GZ-30MIN`
- Added new filetype `DAV12-RAW-FF3-TBL1-201412041850-CSV-GZ-10MIN`
- Added new filetype `DAV12-RAW-FF3-TBL1-201802281110-TOA5-DAT-10MIN`
- Added new filetype `DAV12-RAW-FF3-TBL1-201807181512-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF3-TBL2-201507021850-CSV-GZ-25H`
- Added new filetype `DAV12-RAW-FF3-TBL2-201802281504-TOA5-DAT-25H`

- Added new filetype `DAV12-RAW-FF4-TBL1-201410231208-CSV-GZ-30MIN`
- Added new filetype `DAV12-RAW-FF4-TBL1-201412041850-CSV-GZ-10MIN`
- Added new filetype `DAV12-RAW-FF4-TBL1-201802281110-TOA5-DAT-10MIN`
- Added new filetype `DAV12-RAW-FF4-TBL1-201807181521-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF4-TBL1-201903041641-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF4-TBL2-201507021850-CSV-GZ-25H`
- Added new filetype `DAV12-RAW-FF4-TBL2-201803011804-TOA5-DAT-25H`

- Added new filetype `DAV12-RAW-FF5-TBL1-201410231208-CSV-GZ-30MIN`
- Added new filetype `DAV12-RAW-FF5-TBL1-201412041850-CSV-GZ-10MIN`
- Added new filetype `DAV12-RAW-FF5-TBL1-201802281100-TOA5-DAT-10MIN`
- Added new filetype `DAV12-RAW-FF5-TBL1-201807181525-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF5-TBL1-201903041640-TOA5-DAT-1MIN`
- Added new filetype `DAV12-RAW-FF5-TBL2-201507021850-CSV-GZ-25H`
- Added new filetype `DAV12-RAW-FF5-TBL2-201802281904-TOA5-DAT-25H`

- Added new filetype `DAV12-RAW-FF6-TBL2-202310261047-TOA5-DAT-1MIN`

- Checked depths for DAV12 TBL1 files, some depths were fixed (slightly wrong)

### More additions

- Added more variables in `DAV10-RAW-TBL1-201807181036-TOA5-DAT-10S`
- Added more variables in `CHA10-RAW-TBL2-202001281750-TOA5-DAT-10MIN`

### Other changes

- Removed setting `data_mangle_dupe_cols` from filetype configs, not supported by pandas in newer versions

## v20230309 | 9 Mar 2023

- Added new parameter `ignore_after` in filetype settings.
  For example: `ignore_after: "2022-05-10 07:56:00"` would ignore all data for the
  respective variable after the given datetime. This option was implemented because
  of overlapping time periods of equally named variables. First implemented for
  filetype `LAE11-RAW-TBL1-202012010000-TOA5-DAT-1MIN.yaml`.

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
