# Driven Data : Pump it Up - Data Mining the Water Table

## Context

This is my work done for the "Pump it up" competition hosted by [Driven Data](www.drivendata.org/competitions/7/page/23/)

**Can you predict which water pumps are faulty?**

"Using data from Taarifa and the Tanzanian Ministry of Water, can you predict which pumps are functional, which need some repairs, and which don't work at all?"

## Dataset

The data for this competition comes from the Taarifa waterpoints dashboard, which aggregates data from the Tanzania Ministry of Water.

More details about the dataset origin on:
- [Taarifa homepage](http://taarifa.org/)
- [Taarifa blog](https://taarifa.wordpress.com/)
- [Taarifa Github](https://github.com/taarifa)

The test dataset is made of 59400 total entries. Each one consisting of 40 features and one pump status (functional / functional needs repair / non functional).

Most the features are text entries (like installer name, funder name, etc.) or categorical entries (extraction type group, payment type, etc.). Only a few feature have numerical values (amount_tsh, population) and some are date related (date_recorded, construction_year).

## Code

**My Jupyter Notebook can be found [here](Taarifa.ipynb)**

Packages versions:

- Python: v2.7.11
- NumPy: v1.10.4
- Pandas: v0.18.0
- sklearn: v0.17.1

## Summary

### Exploratory data analysis

There are sometimes several entries for the same water point. As these entries have different funders and construction years, I guess that they correspond to different pumps. 

Several data quality issues can be spotted:
- the scheme_name feature is showing a high ratio of NaN values, which may be a reason not to use it for the classification algorithm.
- the longitude and latitude features are sometimes entered with values that do not correspond to Tanzanian geographical position
- num_private and recorded_by does not seem to bring any kind of information
- scheme_name is missing a lot of entries
- construction year seems to have some strong outliers (year = 0)

Also, some categorical features include a lot of different category levels (funder, wpt_name, subvillage, installer, ward, scheme_name). In order to use them in my classification algorithm, I will have to reduce this number of category in order to limit the size of my dataset after one-hot-encoding. 

### Feature engineering

In order to reduce the number of categorical features, I only selected the features that seemed relevant to me to predict the pump status. I based my selection on the previous exploratory data analysis. 

I had to deal with missing data and outliers, and I also had to reduce the number of category levels for some categorical features. To do so, I selected the 20 most frequent items and set all the rest to "other".

Before using the dataset with classification algorithms, I implemented one-hot encoding on categorical features. 

The following features will be used to train the classification algorithm:

feature   |   type   |   unique values   |   observation
---------- | ---------- | ---------- | ----------
amount_tsh | numerical | | high number of values set to zero
date_recorded_delta | numerical (time difference) | |
funder | categorical | 1897 | missing values set to "0", category level reduced to 20
gps_height | numerical | | high number of values  set to zero
installer | categorical | 2145 | missing values  set to "0", category level reduced to 20
longitude | numerical | | missing or zero values set to average
latitude | numerical | | missing or zero values set to average
basin | categorical | 9 | 
region | categorical | 21 | 
district_code | categorical | 20 | 
lga | categorical | 125 | 
population | numerical | | high number of values set to zero
public_meeting | categorical | 2 | 
scheme_management | categorical | 12 | 
permit | numerical |  | missing values set to average
construction_year | date | | zero and missing values set to average
extraction_type | categorical | 18 | 
management | categorical | 12 | 
payment | categorical | 7 | 
water_quality | categorical | 8 | 
quantity | categorical | 5 | 
source | categorical | 10 | 
waterpoint_type | categorical | 7 | 

### Classification algorithm and results

The main classification algorithm explored for now is Random Forest. Combined with a grid search approach and a stratified shuffle split on the training set, I was able to get a decent classifier that scored an accuracy of 0.808 on the cross-validation set.

After submitting my predictions, I scored 0.815 which brings me to the 167th rank on 2438. 

## Limits and potentials

I see three areas where I could improve my work:
- when reducing the number of category levels, I could regroup them depending on their pump status probability. As an example, all the categories with a high probability of non functional pumps would be grouped together and identified as one category. 
- I should determine if one specific pump status has a significantly low accuracy score with my classifier and try determine what would be the reason for that.
- I should try other classification algorithms, like XGBoost, or SVM and maybe combine different models for ensemble learning. I should also try to do some further hyperparameter tuning.

## License

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org>


