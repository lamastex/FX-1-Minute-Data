# FX 1-Minute Dataset

Retrieval made easy.

## Data specification

This repository contains:
- A dataset of all the FX prices (1-minute data). from 2000 to late June 2019.
- A set of functions to download the historical prices yourself.

All the data is retrieved from: http://www.histdata.com/

Any file in a dataset is zipped and contains: 
- a CSV (semicolon separated file).
- a status report (containing some meta data such as gaps).

Any CSV file looks like this:

```bash
20120201 000000;1.306600;1.306600;1.306560;1.306560;0
20120201 000100;1.306570;1.306570;1.306470;1.306560;0
20120201 000200;1.306520;1.306560;1.306520;1.306560;0
20120201 000300;1.306610;1.306610;1.306450;1.306450;0
20120201 000400;1.306470;1.306540;1.306470;1.306520;0
[...]
```

Headers are not included in the CSV files. They are:

```bash
DateTime Stamp;Bar OPEN Bid Quote;Bar HIGH Bid Quote;Bar LOW Bid Quote;Bar CLOSE Bid Quote;Volume
```

### DateTime Stamp

Format:
`YYYYMMDD HHMMSS`

Legend:
- YYYY – Year
- MM – Month (01 to 12)
- DD – Day of the Month
- HH – Hour of the day (in 24h format)
- MM – Minute
- SS – Second, in this case it will be always 00

TimeZone: Eastern Standard Time (EST) time-zone *WITHOUT* Day Light Savings adjustments

### OPEN Bid Quote

The open (first) bid quote of the 1M bin.

### HIGH Bid Quote

The highest bid quote of the 1M bin.


### LOW Bid Quote

The lowest bid quote of the 1M bin.

### CLOSE Bid Quote

The close (last) bid quote of the 1M bin.

### Volume

Number of lots. From what I saw it's always 0 here.

## Data files provided: Early 2000 to May 2017

Available [2000-Jun2019/](2000-Jun2019)

## How to download your own dataset?

This command will re-download all the FULL FX dataset up to today (expect the runtime to be ~4 hours).

```bash
pip install -r requirements.txt
python download_all_fx_data.py
```

## API

Then of course, you can use directly the API. There are two endpoints depending on what you query:
- If you query data of the same year, then you have to query it per month. Say, you are in June 2019. You will need to call to make 6 calls to download the data of 2019 up to June.

```python
api.download_fx_m1_data(year='2016', month='7', pair='eurgbp')
```

- If you query data for past years, then you can query the whole year in one call.

Use this function for downloading data related to the CURRENT year. E.g. if you're interested in data of 2017 and we're in 2017, use this one.

```python
api.download_fx_m1_data_year(year='2016', pair='eurgbp')
```

