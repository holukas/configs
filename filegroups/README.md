... in progress ...

# Filegroups

`dataflow` uses `yaml` files to collect settings for different filetypes.
Settings comprise information how the respective data file
Overview of the different settings in the filegroups `yaml` files.

## Filetype Identifier (ID)

The `yaml` files start with the filetype identifier (ID) at the top.

The ID can be anything, there are not restrictions, but current IDs were named in a
way that aim to give the most essential info for this type.

For example, the ID `DAV13-RAW-NABEL-CSV-1MIN-201901010000` gives information
about the site (`DAV`), filegroup (`13`, which stands for `13_meteo_nabel`),
data type (`RAW` for raw data), origin or data provider (`NABEL`), file delimiter
or file extension (`CSV`), time resolution (`1MIN`) and finally the starting datetime
when first files of this type are considered (`201901010000`). This starting datetime
is set in relation to the date and time info found in the *filename*. In this example,
with the starting datetime being `201901010000`, a file named `DAV_Meteo_NABEL_190101.CSV`
would be valid for this filetype, but not a file `DAV_Meteo_NABEL_181231.CSV`. However,
the starting datetime from the ID is not used to check the datetime validity of datafiles,
but this is done with the settings below.

## Filetype settings



  
  filegroup: 10_meteo
  data_raw_freq: 10T
  data_skiprows: [ 0, 3 ]
  data_headerrows: [ 0, 1 ]
  data_index_col: 0
  data_parse_dates: [ 0 ]
  data_date_parser: '%Y-%m-%d %H:%M:%S'
  data_build_timestamp: false
  data_keep_good_rows: false
  data_remove_bad_rows: false
  data_na_values: [ -9999, nan, NaN, NAN, -6999, '-' ]
  data_encoding: utf-8
  data_delimiter: ','
  data_keep_date_col: false
  data_version: raw
  data_vars_parse_pos_indices: true

### can_be_used_by_filescanner

`true` or `false`

Defines whether the filetype is "seen" during the automatic execution
of the `filescanner` script. Useful to exclude certain files during automatic
uploads to the database. For example, final flux calculations are uploaded
manually (on-demand) to the database, but the results files are still stored on
the server but should be ignored during the daily automatic data upload of other datafiles. 

### filetype_valid_from

datetime in the format `YYYY-MM-DD hh:mm:ss`

The date/time info is read *from the filename* and then checked against
this setting. It is assumed that the date/time info in the filename
gives the starting date/time of the file. If the date/time from the
filename is **later or equal** to this setting, the file is valid for the
respective filetype.

- Example: `filetype_valid_from: 2019-01-01 00:00:00`
- `siteFile_20190419.CSV` is valid
- `siteFile_20181231.CSV` is NOT valid

### filetype_valid_to

datetime in the format `YYYY-MM-DD hh:mm:ss`

The date/time info is read *from the filename* and then checked against
this setting. It is assumed that the date/time info in the filename
gives the starting date/time of the file. If the date/time from the
filename is **earlier or equal** to this setting, the file is valid for the
respective filetype.

- Example: `filetype_valid_to: 2019-06-15 23:59:59`
- `siteFile_20190419.CSV` is valid
- `siteFile_20190727.CSV` is NOT valid

### filetype_id

string to identify files of this filetype

Example: `FILE_*.dat` for the file `FILE_20231201-1450.dat`

### filetype_dateparser

parsing string to parse datetime info from filename

Example: `FILE_%Y%m%d-%H%M.dat` for the file `FILE_20231201-1450.dat`

### filetype_gzip

`true` or `false`; select `true` to directly use `.gz` compressed files





`RAWVAR: { field: VAR, units: UNITS, gain: 1, rawfunc: [ calc_lw, PT100_2_AVG, LWin_2_AVG, LW_IN_T1_2_1 ], measurement: _instrumentmetrics } # rawfunc`

- `RAWVAR` ... name of original raw data variable, e.g. `PT100_2_AVG`
- `VAR` ... name of renamed variable, following naming convention, e.g. `T_RAD_T1_2_1`
- `UNITS` ... units of `VAR`, after applying `gain`, e.g. `degC`
- `gain` ... float or integer; gain applied to `VAR` before uploading to the database, e.g. `0.001`
- `rawfunc` ... list; function executed on raw data to produce a new variable, e.g. for the calculation of
  `LW_IN_T1_2_1` from `PT100_2_AVG` and `LWin_2_AVG`, using the function `calc_lwin`.

Example:
`PT100_2_AVG: { field: T_RAD_T1_2_1, units: degC, gain: 1, rawfunc: [ calc_lw, PT100_2_AVG, LWin_2_AVG, LW_IN_T1_2_1 ], measurement: _instrumentmetrics } # rawfunc`