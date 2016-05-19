# dlmpanel
A package for Dynamic Linear Model on panel data in Marketing-Mix Modelling

## Installation

You can install `dlmpanel` from `github` using the `devtools` package

```coffee
require(devtools)
install_github('xinzhou1023/dlmpanel')
```
## Example
```coffee
library(dlmpanel)

# load sample data
data(sample_data)
data=sample_data

# fit dlm
fit=DLM(
  spec=rep("localhost",4), # for parallel computation, only change the number which stands for # of threads
  data=data, # modeling dataset, MUST be data.frame class, NOT data.table class
  group="market", # Cross section column name in dataset
  date.var="date", # date column name in dataset
  start.date.m="2007-01-01", # Start date for modeling
  end.date.m="2013-12-31", # End date for modeling
  start.date.c="2012-01-01", # Starting date for computing contribution
  end.date.c="2012-12-31", # Ending date for computing contribution
  clv=263, # CLV value used in contirubtion calculation, if no need to use clv, then set it as 1
  is.output=F, # whether output decomp files
  is.graph=T, # whether graph resutls
  model.name="DDA_EX",  # Name of model, for output files' name
  contrb.date="2012", # The time period of contribution, for output files' name
  
  # Vector of all the independent variable names, if no independent, then put c()
  # Always put variables required NEGATIVE coef at the beginning, and variables which don't need sign check in the end
  varlist=c( 
    "csCHECKING_DDAxx50i",
    "csciCHECKING_DDA_compxx50i",
    
    "Existing_Home_Sales_USxxi",
    #    "affdisthhxxneti",
    #"bawboc_attxxi",
    #    "bransat2",
    "hckttvxx50i",
    "hcktinxx10i",
    "hcktexpxx50i",
    "hcktposxx50i",
    "hcktraxx50i",
    "hhlttoxx50i",
    "hmgttoxx50i",
    "hhettoxx50i",
    "hsvttoxx50i",
    "himttoxx50i",
    "hmucsrttvxx_ixADShl30hrf30_i",
    "hmucsrtinxx_ixADShl30hrf30_i",
    "hmucsrtprxx_ixADShl30hrf30_i",
    "hmucsrtotherxx_ixADShl30hrf15_i",
    "hmucbGenttoxx_ixADShl3hrf5_i",
    "hmucbSoluttvxx_ixADShl6hrf15_i",
    "hmucbSolutdpxx_ixADShl10hrf30_i",
    "hmucbSolutsrxx_ixADShl0hrf5_i",
    "hmucbSolu_Other_ixADShl10hrf30_i",
    "hmucbASavttoxx_ixADShl30hrf15_i",
    "hmucbAmerttvxx_ixADShl0hrf5_i",
    "hmucbAmertdpxx_ixADShl0hrf5_i",
    "hmucbAmertsrxx_ixADShl0hrf5_i",
    "hmucbAmer_Other_ixADShl3hrf5_i",
    #   "BAC_Buzz_Proportional10MA",
    #   "BAC_Recommend_Proportional10MA",
    "Card_DM_465i",
    "Checking_Promotion",
    "hurricane_sandy",
    "hol"  
  ),
  
  # Vector of variables above without coef sign constraint, if nothing then put c()
  dum=c(
    "hurricane_sandy",
    "hol" 
  ),
  
  # For the variables in the dum vector above, specify the DMA's on which they would have impact. If nothing then put NA
  dum_matrix=cbind(c(0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0),rep(1,20)),
  
  num_ng_coef=2, # no. of negative coef variables
  dep="chkopens_ext_adji", # dependent var name
  actual="chkopens_ext_adj", # actual var name
  mean="chkopens_ext_adjm",# weitht of actual var name
  
  # DLM setup
  CI.level=0.95, # CI level
  
  iter=100, # no. of iteration for model selection
  
  level_type=2, # Moving base type: 1 represents level type; 2 represents level+slope type
  
  var_coef=0, # Variance of covariate coef: 0 reprensents constant coef, non-zero value represents time-varying coef. For instance, 1^2 means the standard deviation of each coef is 1 and they are time-varying coef's.
  # "est" means estimated by model
  
  var_level=5e-3^2,  # Variance of the level part of moving base. 0 reprensents constant level, non-zero value represents time-varying level.
  # "est" means estimated by model
  
  var_slope=0, # Variance of the slope part of moving base. 0 reprensents constant slope, non-zero value represents time-varying slope.
  # "est" means estimated by model
  
  per_sea=52, # Period of seasonality variable. For instance, I'm using weekly data, so my season period is 52 weeks which is one year.
  
  ord_sea=6, # No. of harmonics of seasonality variable. The season var is a Fourier series so that no. of harmonic need to be specified.
  
  var_sea=0, # Variance of the seasonality. 0 reprensents constant seasonality, non-zero value represents time-varying seasonality, which means for each period, seasonality is different.
  # "est" means estimated by model
  
  optim.method="L-BFGS-B", # Optimization method for variance of error estimation. Usually don't need to change.
  
  optim.lb=-Inf, # Lower bound of variance of error for optimization. Usually don't need to change.
  
  optim.ub=Inf # Upper bound of variance of error for optimization. Usually don't need to change.
)

# final coef
fit$coef
# final model statistics
fit$stat
```
