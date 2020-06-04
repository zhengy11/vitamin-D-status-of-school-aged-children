## vitamin-D-status-of-school-aged-children

### Examine the association between vitamin D levels and the two exposure measures using regression methods.

Stata dofile:

```Stata
"""
* Do-file:   vitaminDlevel.do
* Project:   MAST90102 Linear Regression project
*
* Data used:   "vitaminD.dta"
* Data created:  none
* Date:    30 October 2019
*
* Author:    Ying ZHENG
* Purpose:     To create a do file for Linear Regression vitaminDlevel

capture log close 
version 15.1
clear
set more off

cd "~/Documents/Biostatistics semester 2/Linear Regression/VitaminDlevel"
log using vitaminDlevel.log, replace
use vitaminD.dta, clear
```

generate the descriptive statistics

```Stata
"""
codebook
//check the missing data of all variables
describe age female height_age_z bmi_age_z d25
sum age height_age_z bmi_age_z d25
tab female
```
