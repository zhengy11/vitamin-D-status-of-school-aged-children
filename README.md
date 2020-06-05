## Vitamin D status of school-aged children

### Examine the association between vitamin D levels and the two exposure measures using regression methods.

Stata dofile as follow:

```Stata
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

Generate the descriptive statistics

```Stata
//check the missing data of all variables
codebook
describe age female height_age_z bmi_age_z d25
sum age height_age_z bmi_age_z d25
tab female

//check the distribution of continuous variables
hist height_age_z,normal kdensity///Y
hist d25,normal kdensity///Y
hist age,normal kdensity
hist bmi_age_z,normal kdensity
```

Investigation of model assumptions
```Stata
///regress d25 height_age_z female age
regress d25 height_age_z
//check the linearity between outcome and continuous covariates
scatter d25 height_age_z

//generate the residual not the predict value
predict resid_d25, resid 
scatter resid_d25 height_age_z,yline(0)

//superimposed the lowess curve on scatterplot
lowess d25 height_age_z, bwidth(0.5) xtitle("height-for-age z-score") ytitle("Serum 25-hydroxyvitamin D (nmol/L) ")
///lowess d25 age, bwidth(0.5) xtitle("Age (years)") ytitle("Serum 25-hydroxyvitamin D (nmol/L) ")

//Quantile-Quantile plot assessing normality of residuals of d25 on covariates
qnorm resid_d25

//residuals versus fitted values of d25 and covariates
rvfplot, yline(0)

//Check the outliers/influential observations
dfbeta
sum _dfbeta_1, detail
scatter _dfbeta_1 id, mlabel(id) yline(0) ytitle("Dfbeta height-for-age z-score")
```

Fit the linear model
```Stata
gen stunt = 0
replace stunt = height_age_z + 2 if height_age_z >= -2
regress d25 height_age_z stunt
```

Generate the graph of estimated association
```Stata
predict pred_d250 if stunt == 0
predict pred_d251 if stunt != 0
twoway scatter d25 height_age_z if stunt == 0, mcolor("dknavy") || scatter d25 height_age_z if stunt !=0, mcolor("maroon") ///
|| line pred_d250 height_age_z if stunt==0|| line pred_d251 height_age_z if stunt!=0, legend(pos(5) region(fcolor(gs15)) lab(1 "before the threshold") lab (2 "after the threshold") ///
lab(3 "Predicted line") lab(4 "Predicted line")) ///
ytitle("Serum 25-hydroxyvitamin D (nmol/L)") title("Estimated association") subtitle(" between vitamin D level and height-for-age z-score") xtitle("height-for-age z-score")
```
