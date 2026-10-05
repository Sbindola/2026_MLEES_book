# Datasets

Where the files committed under `data/` come from: one section per file, with what was checked
and when, and what would break if the file were replaced.

## `part-I/station_iib_daily_max_temp_2022.csv`

Daily maximum air temperature for 2022 at station IIB, Independence Municipal Airport, Iowa,
US. Used in 1.1 (Exercise 9) and 1.2 (Exercise 16), exercises and solutions.

| Field | Value |
|---|---|
| Station | IIB (ICAO KIIB), Independence Municipal Airport, Buchanan County, Iowa, US |
| Station type | automated weather observing system (AWOS), listed in the Iowa Environmental Mesonet's Iowa ASOS network, `IA_ASOS` |
| Location | 42.4544° N, 91.9504° W, elevation 294 m (IEM station metadata) |
| Variable | daily maximum air temperature, in degrees Fahrenheit |
| Period | 2022-01-01 to 2022-12-31, one row per day, 365 rows |
| Missing values | 16 empty temperature fields, 25 October to 9 November 2022 |
| Format | CSV with CRLF line endings; header `station,day,max_temp_f`; day as `dd.mm.yy` |
| Size | 6797 bytes |
| sha256 | `034755fb289b4e157e5d029995481786a359eb25a80df62c09ee83d063013144` |
| Licence | IEM products are public domain; IEM asks for attribution to the Iowa Environmental Mesonet of Iowa State University |

### Origin

The file comes from the 2025 edition of the course. There, the lab at
<https://freddy0218.github.io/2025_MLEES_book_online/notebook/W1_S1.html> fetched its station
data as a zip from a University of Lausanne SharePoint link, which returned HTTP 403 on
2026-09-23. That page and its tutorial page do not name the station or say where the data came
from. The file was committed here on 2026-08-20 as `data/part-I/Ch1-Lab01-Ex8.csv` and renamed
on 2026-09-15; its bytes are unchanged (same sha256).

### Upstream source

The values match the Iowa Environmental Mesonet (IEM) "Computed Daily Summary of Observations"
for station IIB, variable `max_temp_f`. Compared on 2026-09-23 against a fresh download:

<https://mesonet.agron.iastate.edu/cgi-bin/request/daily.py?network=IA_ASOS&stations=IIB&year1=2022&month1=1&day1=1&year2=2022&month2=12&day2=31&var=max_temp_f&format=csv&na=blank>

- The header, `station,day,max_temp_f`, is identical. IEM writes the day as `yyyy-mm-dd`; this
  file uses `dd.mm.yy`.
- The 16 empty days are the same 16 days in both.
- Of the 349 days with a value, 188 are identical and 229 agree to within 0.1 °F.
- 118 days are 0.2 to 1.3 °F higher in this file than in the current IEM record, and two differ
  by far more: 31 January 2022 (55.0 °F here, 46.6 °F at IEM) and 31 March 2022 (65.1 °F here,
  51.8 °F at IEM).

IEM computes these summaries from the observations it has collected, so the most likely
explanation is that this file was downloaded before IEM recomputed the 2022 values. That is an
inference: when and how the 2025 file was produced is not recorded anywhere found. The station
identity does not rest on that inference: the same 16-day gap and 188 identical values are not
something an unrelated station would reproduce.

### Before replacing it

A fresh IEM download is not this file: the values differ as above, and the day format and line
endings differ too. Replacing it changes the sha256, so the `known_hash` in all four 1.1 and 1.2
notebooks (exercises and solutions) must be updated, and every value quoted in their solution
comments rechecked: the 1.1 solutions quote the first two records, 14.7 °F and 8.6 °F.

Station metadata: <https://mesonet.agron.iastate.edu/sites/site.php?station=IIB&network=IA_ASOS>.
IEM terms of use: <https://mesonet.agron.iastate.edu/disclaimer.php>.

## `part-I/uscrn_daily_2017_ny_millbrook_3w.txt`

One year of daily observations from the U.S. Climate Reference Network (USCRN), station
Millbrook 3 W, New York. Used in 1.5's lecture, section "Reading Data Files: Weather Station Data"
and everything after it.

| Field | Value |
|---|---|
| Station | WBANNO 64756, Millbrook 3 W, New York, US |
| Location | 41.79° N, 73.74° W |
| Period | 2017-01-01 to 2017-12-31, one row per day, 365 rows plus one header line |
| Variables | 28 columns, as listed in NOAA's `HEADERS.txt`: daily air temperature, precipitation, solar radiation, surface temperature, relative humidity, and soil moisture and soil temperature at 5, 10, 20, 50 and 100 cm |
| Missing values | −9999.0 (−99.000 in the three-decimal soil-moisture columns). No data at all on 2017-10-04; soil moisture at 5 cm is missing on 48 days, 43 of them in January and February |
| Format | whitespace-separated text, variable-width columns; dates as `YYYYMMDD` |
| Size | 79650 bytes |
| md5 | `5129dcfd19300eb8d4d8d1673fcfbcb4` (the hash the 2025 edition pinned) |
| sha256 | `f97637cf9909548a10843e9af352470b8aa1f9df5d971aa74e7478d63c7eca07` |
| Licence | the Zenodo mirror is CC-BY-4.0; the underlying NOAA data are a US government product |

### Origin

The 2025 edition's pandas tutorial
(<https://freddy0218.github.io/2025_MLEES_book_online/notebook/W3_S1_Tutorial.html>) fetched
`data.txt` with pooch from the Zenodo record "Mirror of data from NOAA U.S. Climate Reference
Network for Research Computing in Earth Science" (R. Abernathey,
<https://doi.org/10.5281/zenodo.5564850>), pinned by md5. Downloaded from that record on
2026-10-05; the md5 matched. The file was committed here under a descriptive name, bytes unchanged.

### Upstream source

`data.txt` is the mirror's `CRND0103-2017-NY_Millbrook_3_W.txt` with one header line of column
names prepended; below the header the two are byte-identical (checked 2026-10-05). Compared the
same day against NOAA's current file,
<https://www.ncei.noaa.gov/pub/data/uscrn/products/daily01/2017/CRND0103-2017-NY_Millbrook_3_W.txt>:
16 of 365 rows differ, all between 2017-10-06 and 2017-12-30, all in the soil-moisture columns,
and never by more than 0.001 m³ m⁻³. NOAA reprocessed those values after the mirror was made.
Column definitions and the missing-value codes are in
<https://www.ncei.noaa.gov/pub/data/uscrn/products/daily01/README.txt>.

### Before replacing it

A fresh NOAA download has no header line and slightly different soil-moisture values, so
`read_csv` would need `names=` and the hash in 1.5's fetch cell would change. Values quoted in 1.5's
prose — 364 temperature values, 48 soil-moisture gaps, the `fillna(0)` bias — would need
rechecking.
