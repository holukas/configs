# Notes

## Note#1, CH-CHA

Precipitation is corrected with `gain=1.317`. This value is based on the comparison of precipitation measurements from
the Lambrecht 15188 and the Toss 10116 sensor.
This correction factor was also used in the old Python MeteoScreeningTool and applied to precipitation data from the
start of measurements (2005) until `2016-03-07 09:00:00`.
From the precipitation sensor comparison:
The analysis is based on data from 247 days when both sensors measured parallel at the site Chamau (CH-CHA) during the
season 2015. The Toss 10116 sensor is the old rain gauge, the Lambrecht 15188 senor was installed in April 2015. Daily
precipitation sum are compared. Result: The old Toss sensor measures daily precipitation 24% lower than the new
Lambrecht. (r2 = 0.99).
For correction of old data, the correction factor of 1.317 is suggested: `P_new = 1.317 * P_old`

## Note#2, CH-CHA

Raw data contain some wrongly labelled variables for `TS`.
Comment from IF in the CONFIG file of the previous Python meteoscreening tool:
> Depths are switched (wrong name in the logger program) since installation of these sensors. Only 5TMs at GF1, rep 2.

From file: `CH-CHA_CONFIG_Linux_all_years_all_vars_IMPORTANT_KEEP_THIS.xlsx`
Here, new variable names were set accordingly.

## Note#3, CH-CHA

The time period between `2010-03-31 10:30:00` and `2010-07-28 09:30:00` shows negative and generally muted values. This
time period is removed during upload because a complete time series of `PPFD_IN_T1_2_1` is available and should be used
to gap-fill `SW_IN_T1_2_1`.

I found a correction in the source code of the old MeteoScreeningTool. However, the correction does not seem to address
the full issue and leaves open questions, I found no additional documentation how the final 30MIN values were achieved
and I could not reproduce the correction process. However, here is what I found:

I was looking at how the correction for this time period was done previously. If I understood the config settings in the
old Python MeteoScreening tool correctly, then (1) values outside a range of -20/1500 were removed, (2) a conversion was
applied `(measurement / 16.2602) * 183.486`, (3) the nighttime offset was removed, and then (4) all remaining values
below zero were set to zero.

Step (1) might have removed a lot of data points from the time series, because many measured values were below `-20`,
resulting in gaps. I checked the screened 30MIN values for some days during this time period and I think that often the
potential radiation (or maybe PPFD) was used to fill these gaps, resulting in a rather smooth radiation course. The
documented conversion corresponds to a gain of `-11.284363045964993` for the given time period, calculated
as `(1/16.2602) * 183.486 * -1`. The `-1` needs to be applied to convert the measured negative values to positive.

The nighttime offset is not handled by `dataflow`, but later removed during meteoscreening using `diive`.

After applying the rawfunc `apply_gain_between_dates` with gain `-11.284363045964993`, data can later be corrected
during `diive` meteoscreening by applying the offset correction. This generates a somewhat realistic time series, but it
is not perfect, only an attempt to save the data and make it usable.

Comment from the old configs:
> rescale, sensors on multiplexer shifted by 1 DIFF, 2 SE Channels;

So, the correction is not applied, but data are removed and later PPFD is used to fill the gaps.

## Note#4, CH-CHA

SWC is directly calculated from Theta mV signal using the rawfunc, SDP is calculated in a separate step also directly
from the Theta mV signal. The SDP variable is then stored with units "unitless" in the database.
Equation from the old MeteoScreeningTool Excel config file to calculate SDP (unitless) from Theta mV signal:
`SDP_AVG_GF1_0.05_1, (1.07+6.4*(x[0]/1000)-6.4*(x[0]/1000)**2+4.7*(x[0]/1000)**3)**2`
> "calculates SDP from VOLT (mV/1000) measurement. This step got lost, was done in the previous screening though. mV
> range [850,950], SDP range [20,35]. Equation from current loggerprogram where these sensors are still measured.
> Originally from Werner's logger-cleaning.R" 
