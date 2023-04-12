# US Treasuries Yield Curve Data

## Data source

Data source page: ["Daily Treasury Par Yield Curve Rates"](https://home.treasury.gov/resource-center/data-chart-center/interest-rates/TextView?type=daily_treasury_yield_curve&field_tdr_date_value=2022) 

## Read dataset from repo

CSV file (1990-present): [daily UST yields](https://raw.githubusercontent.com/epogrebnyak/data-ust/master/ust.csv)
 
```python
import pandas as pd
url = "https://raw.githubusercontent.com/epogrebnyak/data-ust/master/ust.csv"
df = pd.read_csv(url, parse_dates=["date"]).set_index("date")
```

## Download current dataset

Build a local CSV file with US Treasuries yields by maturity: 

```python
import os

import pandas as pd

from ust import save_xml, read_rates, available_years

# save UST yield rates to local folder for selected years
for year in available_years():
    save_xml(year, folder="./xml")

# run later - force update last year (overwrites existing file)
save_xml(2022, folder="./xml", overwrite=True)

# read UST yield rates as pandas dataframe
df = read_rates(start_year=2020, end_year=2022, folder="./xml")
print(df)

# save as single CSV file
df.to_csv("rates.csv")

# read back later
df = pd.read_csv("rates.csv", parse_dates=["date"]).set_index("date")


"""
            BC_1MONTH  BC_3MONTH  BC_6MONTH  BC_1YEAR  ...  BC_10YEAR  BC_20YEAR  BC_30YEAR  BC_30YEARDISPLAY
date                                                   ...                                                   
2020-01-02       1.53       1.54       1.57      1.56  ...       1.88       2.19       2.33              2.33
2020-01-03       1.52       1.52       1.55      1.55  ...       1.80       2.11       2.26              2.26
2020-01-06       1.54       1.56       1.56      1.54  ...       1.81       2.13       2.28              2.28
2020-01-07       1.52       1.54       1.56      1.53  ...       1.83       2.16       2.31              2.31
2020-01-08       1.50       1.54       1.56      1.55  ...       1.87       2.21       2.35              2.35
...               ...        ...        ...       ...  ...        ...        ...        ...               ...
2022-03-07       0.17       0.38       0.75      1.07  ...       1.78       2.29       2.19              2.19
2022-03-08       0.16       0.36       0.72      1.12  ...       1.86       2.34       2.24              2.24
2022-03-09       0.18       0.38       0.75      1.15  ...       1.94       2.38       2.29              2.29
2022-03-10       0.19       0.39       0.75      1.19  ...       1.98       2.45       2.38              2.38
2022-03-11       0.17       0.40       0.78      1.22  ...       2.00       2.45       2.36              2.36

[550 rows x 12 columns]
```

## Sample image

![](ust.png)

## Notes

- `start_year` can go back to 1990, but even longer dataset is avilable at FRED
- API [changed](https://home.treasury.gov/developer-notice-xml-changes) in February 2022 


