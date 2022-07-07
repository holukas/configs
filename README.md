# configs

This folder contains configuration files needed to run `POET` scripts.
Currently used by the packages [dataflow](https://gitlab.ethz.ch/poet/dataflow) and [dbc](https://gitlab.ethz.ch/poet/dbc).

Note that the database configuration is not stored in `configs`, but in a separate folder 
that has the same name as the `configs` folder but with the suffix `_secret`. If the `configs`
folder is given e.g. in the script `dataflow` as `C:\configs`, then the database configuration
is assumed to be stored in folder `C:\configs_secret`.

## Defined filetypes
*updated: 2022-07-07*
- PROC-EDDYPRO-FLUXNET-CSV-L0-30MIN
- PROC-FLUXNET-FULLSET-HH-CSV-30MIN
- AWS10-RAW-TBL1-201701260236-TOA5-DAT-1MIN
- AWS11-RAW-TBL1-201909190000-TOA5-DAT-30MIN
- AWS12-RAW-TBL1-201909231530-TOA5-DAT-10MIN
- AWS13-RAW-202001010100-CSV-1MIN
- AWS15-RAW-201911130000-TOA5-DAT-1MIN
- CHA10-RAW-TBL1-201612071258-TOA5-DAT-1MIN
- CHA10-RAW-TBL2-202001281750-TOA5-DAT-10MIN
- DAV10-RAW-PA-202112020000-TOA5-DAT-10S
- DAV10-RAW-TBL1-201802281101-TOA5-DAT-10S
- DAV10-RAW-TBL2-201808281430-TOA5-DAT-1MIN
- DAV11-RAW-TBL1+-201108250000-TOA5-DAT-10MIN
- DAV11-RAW-TBL2-201804051700-TOA5-DAT-1H
- DAV11-RAW-TBL3-201812060001-TOA5-DAT-1H
- DAV12-RAW-FF1-TBL1-201802281110-TOA5-DAT-10MIN
- DAV12-RAW-FF1-TBL1-201807181445-TOA5-DAT-1MIN
- DAV12-RAW-FF1-TBL1-201903050001-TOA5-DAT-1MIN
- DAV12-RAW-FF1-TBL1-202110221616-TOA5-DAT-1MIN
- DAV12-RAW-FF1-TBL2-201802281104-TOA5-DAT-25H
- DAV12-RAW-FF2-TBL1-201903070001-TOA5-DAT-1MIN
- DAV12-RAW-FF2-TBL1-202110221618-TOA5-DAT-1MIN
- DAV12-RAW-FF3-TBL1-201903041642-TOA5-DAT-1MIN
- DAV12-RAW-FF3-TBL1-202110221627-TOA5-DAT-1MIN
- DAV12-RAW-FF4-TBL1-202006241033-TOA5-DAT-1MIN
- DAV12-RAW-FF5-TBL1-202006240958-TOA5-DAT-1MIN
- DAV12-RAW-FF6-TBL1-202107292014-TOA5-DAT-1MIN
- DAV13-RAW-TBL1-201809271725-TOA5-DAT-10S
- DAV13-RAW-199701010000-NABEL-SSV-TXT-30MIN
- DAV13-RAW-200001010000-NABEL-SSV-TXT-10MIN
- DAV13-RAW-200901010000-NABEL-SSV-TXT-10MIN
- DAV13-RAW-201601010000-NABEL-SSV-TXT-10MIN
- DAV13-RAW-201901010000-NABEL-CSV-1MIN
- DAV15-RAW-201911050000-ICOS-DAT-1MIN
- DAV15-RAW-202112020000-TOA5-DAT-1MIN
- DAV17-RAW-201807110000-ICOS-PRF-DAT-10S
- DAV17-RAW-202201040000-TOA5-PRF-DAT-10S
- DAV17-RAW-AUX-202112140000-TOA5-PRF-DAT-1MIN
- DAV17-RAW-P2-200001010000-NABEL-PRF-SSV-DAT-5MIN
- DAV17-RAW-P2-200601010000-NABEL-PRF-SSV-DAT-5MIN
- DAV30-RAW-201808280000-ICOSSEQ-PRF-DAT-1S
- DAV30-RAW-201910030000-ICOSSEQ-PRF-QCL-DAT-1S
- DAV40-RAW-201904100000-ICOSSEQ-CMB-DAT-1S
- DAV40-RAW-201910240000-ICOSSEQ-CMB-QCL-DAT-1S
- FRU10-RAW-TBL1-201711201643-TOA5-DAT-1MIN
- FRU10-RAW-TBL2-202112161520-TOA5-DAT-10MIN
- LAE10-RAW-TBL1-201701230926-TOA5-DAT-1MIN
- LAE12-RAW-TBL1-202103291559-TOA5-DAT-1MIN
- OE210-RAW-TBL1-201703150939-TOA5-DAT-1MIN