# configs

This folder contains configuration files needed to run `POET` scripts. Currently used by the  
packages [dataflow](https://github.com/holukas/dataflow) and [dbc-influxdb](https://github.com/holukas/dbc-influxdb).

Note that the database configuration is not stored in `configs`, but in a separate folder that has the same name as    
the `configs` folder but with the suffix `_secret`. If the `configs` folder is given e.g. in the script `dataflow`
as `C:\configs`, then the database configuration is assumed to be stored in folder `C:\configs_secret`. The `_secret`
folder only contains one single file `dbconf.yaml` with the following info:

```  
url: <URL AND PORT OF DATABASE>  
token: <TOKEN FROM INFLUXDB>  
org: <ORG>  
```  

## dirs.yaml

- **Basedirs network addresses, from Windows**: server locations for source data dirs, used to search for source data  
  when `dataflow` is run locally on-demand from a Windows computer, therefore locations are given as SMB
- **Basedirs from gl-calcs**: mounts for source data dirs on the `gl-calcs` Linux computer, used to search for source  
  data when `dataflow` is run automatically on `gl-calcs`, therefore locations are given as folder path
- **Output dirs**: dirs for writing log files and other info output (csv files)
- **Sites subfolders**: names of the site-specific subfolders

## units.yaml

- Maps found units to a standardized name
- For example, `°C` will be changed to `degC`, `W/meter²` is changed to `W m-2`, etc...
- Generally, the key on the left is changed to the value on the right
- Some variables have `false` as value, this means the respective units will not be changed but kept as is.  
  Example: `degC`

## filegroups folder

- In general, the hierarchy is:
    - `configs` > `filegroups` > `<datatype>` > `<site>` > `<filegroup>` > `<filetype>`
    - `filegroups` ... name of the folder in the repo
    - `<datatype>` ... `raw` or `processing`
    - `<site>` ... name of the site, e.g. `ch-aws`
    - `<filegroup>` ... `10_meteo`, `12_meteo_forestfloor`, etc. Filegroups correspond to the subfolders where we
      store  
      respective data files on the server and acts as an additional identifier to group the various filetypes.
    - `<filetype>` ... file that defines data structures of specific files
- To give an example, the filetype `FRU10-RAW-TBL1-201711201643-TOA5-DAT-1MIN.yaml` is defined in location:
    - `configs` > `filegroups` > `raw` > `ch-fru` > `10_meteo` > `FRU10-RAW-TBL1-201711201643-TOA5-DAT-1MIN.yaml`

## Filetype configs

- `filetypes` define how the respective raw data files are handled

### Filetype Identifier (ID)

- The `yaml` files start with the filetype identifier (ID) at the top.
- The ID can be anything, there are not restrictions, but current IDs were named in a way that aim to give the most  
  essential info for this type.
- For example, the ID `DAV13-RAW-NABEL-201901010000-CSV-1MIN` gives information about
    - the site (`DAV`),
    - filegroup (`13`, which stands for `13_meteo_nabel`),
    - data type (`RAW` for raw data),
    - origin or data provider (`NABEL`),
    - the starting datetime when first files of this type are considered (`201901010000`). This starting datetime is
      set  
      in relation to the date and time info found in the *filename*. In this example, with the starting datetime  
      being `201901010000`, a file named `DAV_Meteo_NABEL_190101.CSV` would be valid for this filetype, but not a  
      file `DAV_Meteo_NABEL_181231.CSV`. However, the starting datetime from the ID is not used to check the datetime  
      validity of datafiles, but this is done with the settings below,
    - file delimiter or file extension (`CSV`) and finally the
    - time resolution (`1MIN`).
- The settings for this filetype are listed next.

#### can_be_used_by_filescanner

- `true` or `false`
- Defines whether the filetype is "seen" during the automatic execution of the `filescanner` script. Useful to exclude  
  certain files during automatic uploads to the database. For example, final flux calculations are uploaded manually (  
  on-demand) to the database, but the results files are still stored on the server but should be ignored during the  
  daily automatic data upload of other datafiles.

#### filetype_valid_from

- datetime in the format `YYYY-MM-DD hh:mm:ss`
- The date/time info is read *from the filename* and then checked against this setting. It is assumed that the
  date/time  
  info in the filename gives the starting date/time of the file. If the date/time from the filename is **later or
  equal  
  ** to this setting, the file is valid for the respective filetype.
- Example: `filetype_valid_from: 2019-01-01 00:00:00`
- `siteFile_20190419.CSV` is valid
- `siteFile_20181231.CSV` is NOT valid

#### filetype_valid_to

- datetime in the format `YYYY-MM-DD hh:mm:ss`
- The date/time info is read *from the filename* and then checked against this setting. It is assumed that the
  date/time  
  info in the filename gives the starting date/time of the file. If the date/time from the filename is **earlier or  
  equal** to this setting, the file is valid for the respective filetype.
- Example: `filetype_valid_to: 2019-06-15 23:59:59`
- `siteFile_20190419.CSV` is valid
- `siteFile_20190727.CSV` is NOT valid

#### filetype_id

- string to identify files of this filetype
- Example: `FILE_*.dat` for the file `FILE_20231201-1450.dat`

#### filetype_dateparser

- parsing string to parse datetime info from filename
- Example: `FILE_%Y%m%d-%H%M.dat` for the file `FILE_20231201-1450.dat`

#### filetype_gzip

- `true` or `false`
- select `true` to directly use `.gz` compressed files

#### filegroup:

- string
- Example: `13_meteo_nabel`

#### data_raw_freq

- string that describes the (nominal) time resolution of data files, e.g. `30T`
- can be a list of strings for `-ALTERNATING-` filetypes, e.g. `[ 30T, irregular ]`
- follows the convention of  
  the `pandas` [period aliases](https://pandas.pydata.org/docs/user_guide/timeseries.html#period-aliases)
- `T` for 1MIN time resolution, `30T` for 30MIN time resolution, `1H` for hourly etc...

#### data_skiprows

- `false` or `list` of `int` or empty `list`
- defines which rows should be ignored
- typically used to ignore rows at the start of the files
- important in connection with `data_headerrows`
- Example: `[ ]` to not ignore any row
- Example: `[ 0, 3 ]` to ignore the first and fourth rows in the file

#### data_headerrows

- `list` of `int`, can be `false`
- defines where to find the header of the file
- defines where to find info about variables and units
- This is typically `[ 0, 1]` if the files contain variable names (first row) and units (second row), or `[ 0 ]` if
  the  
  files contain only variable names (first row).
- Is `false` if the file does not contain any header row, this is the case especially for older files.

#### data_timestamp_column

- `int` or `str` or `-9999`
- location of the timestamp column
- Example: `0` if the timestamp is found in the first column of the file
- Example: `TIMESTAMP_END` if the timestamp is found in the column with this name
- Example: `-9999` if there is no timestamp info in the file. Instead, in this case the timestamp has to be
  constructed  
  from other available time/date info using a method defined in `data_build_timestamp`.

#### data_timestamp_timezone_offset_to_utc_hours

- `int`
- offset of the timestamp in relation to UTC
- important because the database stores all data in UTC
- Example: `1` sets the data timestamp index to timezone `UTC+01:00`, which corresponds to CET. Note that the
  timestamp  
  per se is not altered, only the timezone info is added.

#### data_timestamp_format

- `string` or `false`
- parsing string to parse the datetime info
- Examples: `'%Y-%m-%d %H:%M:%S'`, `'%Y-%m-%d %H:%M:%S.%f'`
- `false` if there is no timestamp info in the file. Instead, in this case the timestamp has to be constructed from  
  other available time/date info using a method defined in `data_build_timestamp`.

#### data_build_timestamp

- `false` if there is a timestamp in the file and the timestamp column can be parsed
- If there is no timestamp column in the file, a timestamp can be constructed with:
    - `"YEAR0+MONTH1+DAY2+HOUR3+MINUTE4"` to build the timestamp from columns that give the year (first column, column  
      index 0), month (second column), day (third column), hour (fourth column) and minutes (fifth column, column
      index  
      4).
    - `"YEAR+DOY+TIME"` to build the timestamp from the columns `YEAR`, `DOY` and `TIME`.
    - In these cases the `data_timestamp_column` must be `-9999` because there is no index column in these data files.
    - In these cases `data_timestamp_format` must be `false` because there is no index column in these data files.

#### data_keep_good_rows

- `list` of `int` or `false`
- some files have an identifier in the first column that identifies good data rows
- this setting was introduced because some data files stored data from different data sources in the same file-
- `[ 0, 104 ]` keeps all data rows where the data row starts with `104`, whereby `0` means that the `104` is searched
  in  
  the first column
- `[ 0, 102, 202 ]` keeps all data rows where the data row starts with `102` *or* `202`, whereby `0` means that `102`  
  and `202` are searched in the first column. In this case the variables for ID `102` are described in `data_vars`,
  for  
  ID `202` in `data_vars2`. Files with this setting produce two dataframes, one for each ID.
- Different IDs can have different time resolutions, see setting `data_raw_freq`.
- Yes this makes a lot much more confusing, doesn't it?

#### data_remove_bad_rows

- `false` in almost all cases
- However, there were filetypes where this setting was necessary to ignore unconventional data rows.
- The setting `[ 0, "-999.9000-999.9000-999.9000-999.9000-999.9000"]` was used for  
  filetypes `DAV17-RAW-P2-200001010000-NABEL-PRF-SSV-DAT-5MIN` and `DAV17-RAW-P2-200601010000-NABEL-PRF-SSV-DAT-5MIN`
  to  
  ignore irregular data rows.

#### data_na_values

- `list`
- defines which values to interpret as NAN (not a number, i.e. missing data)
- currently `[ -9999, nan, NaN, NAN, -6999, '-' ]` for all files
- during `dataflow` script execution there are some other safeguards implemented regarding NANs, e.g. some files have  
  the strings `inf` and `-inf` in their data that which are then removed during runtime. These two strings are  
  interpreted as number for whatever reason and cannot be included in the `data_na_values` setting, I think because if  
  they are added as strings here then they are interpreted as strings, but in reality they are a number to Python.  
  Something along these lines...

#### data_special_format

- `false` or `string`
- `"-ALTERNATING-"` identifies special formats that store data from multiple data sources, see  
  also `data_keep_good_rows`
- `"-ICOSSEQ-"` identifies special formats that store data in the [ICOS](https://www.icos-cp.eu/) long-form format
- Data that have a special format are converted to a more regular format during the execution of `dataflow`.
- These data formats can also be identified from the filetype ID,  
  e.g., `DAV10-RAW-PROFILE-200811211210-ALTERNATING-A-10MIN`.

#### data_encoding

- `utf-8` in almost all cases
- `cp1252` is used for `DAV13-RAW-NABEL-*` files, see [here](https://en.wikipedia.org/wiki/Windows-1252) for an  
  explanation about this encoding.

#### data_delimiter

- `','` in most cases
- `';'` for some files
- `'\s+'` for NABEL files, e.g., `DAV17-RAW-P2-200001010000-NABEL-PRF-SSV-DAT-5MIN`

#### data_keep_date_col

- `false` in all cases so far
- means that the original datetime column(s) used to parse or construct the timestamp is removed

#### data_version

- `string`, used to describe the version of the data
- `raw`: raw data
- `eddypro_level-0`: Level-0 (preliminary) flux data,  
  see [Flux Processing Chain](https://www.swissfluxnet.ethz.ch/index.php/data/ecosystem-fluxes/flux-processing-chain/)
- `eddypro_level-1`: Level-1 flux data,  
  see [Flux Processing Chain](https://www.swissfluxnet.ethz.ch/index.php/data/ecosystem-fluxes/flux-processing-chain/)
- `fluxnet_ww2020`: [FLUXNET/ICOS Warm Winter 2020 ecosystem eddy covariance flux product release 2022-1](https://www.icos-cp.eu/data-products/2G60-ZHAK)
- `meteoscreening_mst`: quality-screened meteo data, using the old Python MeteoScreeningTool
- `meteoscreening_diive`: quality-screened meteo data, using `diive`, e.g. using its meteoscreening notebooks
- more will be added in the future

#### data_vars_parse_pos_indices

- `true` for raw data variables that typically contain info about their location in the (standardized) variable name,  
  e.g.. `TA_T1_2_1` is the air temperature on the main tower (`T1`), at `2` m height above ground, replicate `1`. This  
  location info is parsed and then stored as separate tags alongside the variable in the database. `T1` is stored  
  as `hpos` (horizontal position), `2` as `vpos` (vertical position) and `1` as `repl` (replicate number).
- `false` for data that do not have position indexes, e.g., flux calculation simply output the calculated variable.
- This behavior can be overriden for specific variables for which position indices are available, by
  setting `parse_pos_indices: true` for the respective variables.

#### data_vars_order

- `string`
- `free` means that the variables listed under `data_vars` are listed in no particular order, the variable names
  appear  
  in the files.
- `strict`  means the variables listed in `data_vars` are listed in sequence and the sequence must not be changed  
  because the files do not contain variable names. The variable names are directly taken from the `data_vars`.

#### data_vars

- Gives info about the variables found in the file with the format:
    - `<RAWVAR>: { field: <VAR>, units: <UNITS>, measurement: <MEASUREMENT> }`
        - `<RAWVAR>` ... name of original raw data variable, e.g. `PT100_2_AVG`
        - `<VAR>` ... name of renamed variable, following naming convention, e.g. `T_RAD_T1_2_1`
        - `<UNITS>` ... `false` if units are given in data file, otherwise a string e.g. `degC`; units of `VAR`,
          *after*  
          applying `gain`, e.g. `degC`
    - There are some optional parameters that can be  
      used: `<RAWVAR>: { field: <VAR>, units: <UNITS>, gain: <GAIN>, offset: <OFFSET>, parse_pos_indices: <PARSE_POSITION_INDICES>, rawfunc: <RAWFUNC>, measurement: <MEASUREMENT> }`
        - `<GAIN>` ... OPTIONAL gain (`float`) that is applied to `<RAWVAR>` before upload to the  
          database, `<UNITS>` describes the units of `<RAWVAR>` *after* the application of `<GAIN>`. Assumed `1.0` (  
          float) if not given. Typically used to e.g. convert soil water content from `m3 m-3` to `%` by  
          applying `gain: 100.0` (float).
        - `<OFFSET>` ... OPTIONAL offset (`float`) that is applied to `<RAWVAR>` before upload to the  
          database, `<UNITS>` describes the units of `<RAWVAR>` *after* the application of `<OFFSET>`. Assumed `0.0` (  
          float) if not given. Example: `offset: 14.0` (float).
        - `<PARSE_POSITION_INDICES>` ... OPTIONAL, `true` or `false`
          If `true`, parses the position indices from the respective variable, using the name provided in `<VAR>`, even
          if the setting `data_vars_parse_pos_indices` (at the file-level, see above) is set to `false`. For example,
          for the variable `SWC_GF1_0.1_2` the following position indices are parsed: horizontal position `GF1`,
          vertical position `0.1` and replicate `2`. This setting is useful for files that do not contain position
          indices for *all*, but for *some* variables.
          Is assumed `false` if not given.
        - `<RAWFUNC>` ... OPTIONAL list; function executed on raw data to produce a new variable, e.g. for the  
          calculation of `LW_IN_T1_2_1` from `PT100_2_AVG` and `LWin_2_AVG`, using the function `calc_lwin`. The  
          relevant function is defined in the Python script [dataflow](https://github.com/holukas/dataflow).  
          Important: `rawfunc: <RAWFUNC>` must not be given if no rawfunc is required, this means
          that `rawfunc: false`  
          will not work. Currently there are some rawfuncs defined where they were needed, see the *List of rawfuncs*  
          below. The currently implemented functions are shown in  
          the [dataflow repo here](https://github.com/holukas/dataflow/tree/main/dataflow/rawfuncs).

##### List of rawfuncs

- **Calculate long-wave incoming radiation (`LW_IN`)**:  
  `LWin_1_AVG: { field: LW_IN_RAW_T1_2_1, units: false, rawfunc: [ calc_lw, PT100_1_AVG, LWin_1_AVG, LW_IN_T1_2_1 ], measurement: _RAW }`  
  The rawfunc `calc_lw` is used to calculate the new variable `LW_IN_T1_2_1` from the available raw data  
  variables `PT100_2_AVG` (temperature of the radiation sensor in °C) and `LWin_1_AVG` (raw signal of LW_IN in mV).
- **Calculate soil water content (`SWC`) from SDP**:  
  `Theta_11_AVG: { field: SDP_GF1_0.05_1, units: mV, rawfunc: [ calc_swc ], measurement: SDP }`  
  The rawfunc `calc_swc` is used to calculate the new variable `SWC` from `SDP` (soil dielectric permittivity,  
  unitless), whereby in this example the original raw name for `SDP` is called `Theta_11_AVG`. The calculation is  
  site-specific, `dataflow` checks the site and then applies the correct function to run `calc_swc`.
- **Temperature-correction for `O2` measurements**:  
  `O2_GF4_0x1_1_Avg: { field: O2_GF4_0.1_1, units: false, rawfunc: [ correct_o2, O2_GF4_0x1_1_Avg, TO2_GF4_0x1_1_Avg ], measurement: O2 }`  
  The rawfunc `correct_o2` is used to calculate temperature-corrected *soil* O2 (in %) from the original O2  
  measurement `O2_GF4_0x1_1_Avg` (in %) and `TO2_GF4_0x1_1_Avg` (temperature of the O2 sensor in degC). The
  calculation  
  is site-specific, `dataflow` checks the site and then applies the correct function to run `correct_o2`. The original  
  O2 measurement is replaced by the corrected version.
    - **Apply gain between dates**:  
      `SHF_2_AVG: { field: G_GF1_0.03_2, units: W m-2, gain: 1.0, rawfunc: [ apply_gain_between_dates, "2010-03-31 10:30:00", "2010-07-28 09:30:00", 1.0115667782544568 ], measurement: G }`  
      The rawfunc `apply_gain_between_dates` is used to apply gain `1.0115667782544568` to the variable `SHF_2_AVG`,
      but  
      only between the provided dates, in this case all values between `2010-03-31 10:30:00` and `2010-07-28 09:30:00`  
      are multiplied by the gain. The dates include the time and are inclusive (gain is applied also to the provided  
      start and stop dates). Important: for this rawfunc the regular gain of the respetive time series also needs to
      be  
      provided as float, here `gain: 1.0`. The original measurement `SHF_2_AVG` is replaced by the corrected values
      and  
      is stored to the database as `G_GF1_0.03_2`.
    - **Add offset between dates**:  
      `TS_GF1_0x40_1: { field: TS_GF1_0.4_1, units: false, offset: 0.0, rawfunc: [ add_offset_between_dates, "2018-11-04 17:59:00", "2018-12-20 10:33:00", 52 ], measurement: TS }`  ``  
      The rawfunc `add_offset_between_dates` is used to add offset `52` to the variable `TS_GF1_0x40_1`, but only  
      between the provided dates, in this case the offset is added to all values between `2018-11-04 17:59:00`  
      and `2018-12-20 10:33:00`. The dates include the time and are inclusive (offset is added also to the provided  
      start and stop dates). Important: for this rawfunc the regular offset of the respetive time series also needs to  
      be provided as float, here `offset: 0.0`. The original measurement `TS_GF1_0x40_1` is replaced by the corrected  
      values and is stored to the database as `TS_GF1_0.4_1`.

#### data_vars2

- same structure as `data_vars`