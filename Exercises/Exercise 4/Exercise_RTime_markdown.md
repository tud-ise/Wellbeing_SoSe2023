RescueTime
================

# RescueTime

*RescueTime* is a tool to track and organize your screen time. We will
use *RescueTime* and its API to receive our own data set, which we will
analyze during this course. All data you will provide to this course
will be anonymized as well.

For more information about *RescueTime* please visit
<https://www.rescuetime.com/>

You can create an account using the “Try for free” button on the main
page or via <https://www.rescuetime.com/rtx/signup>

*RescueTime* changed some of its interfaces to a new version. If you log
in, you will be forwarded to the new site, but if you want, you can
still use <https://www.rescuetime.com/dashboard> to have access to the
old version, which provides some more information.

## RescueTime R Wrapper

The goal of our *RescueTime* Wrapper is to provide access to
*RescueTime* Data through R. Also, it is possible to get anonymized data
from here.

## Installation

You can install the version via the remotes package:

First, install the “remotes” package in R (RStudio: Tools -> Install
Packages… -> remotes *or* `install.packages("remotes")`), then execute:

``` r
remotes::install_github(repo = "tud-ise/rescuetime-r-wrapper")
```

## Example

After loading the library, you have two ways to access the *RescueTime*
API (get_rescue_time_data() & get_rescue_time_data_anonymized()). You
can get your API Key here: <https://www.rescuetime.com/anapi/manage> The
parameters are the same for both functions. You need to supply your
*RescueTime* API Key and the start and end date as strings. The dates
have to be in ISO Format (YYYY-mm-DDTHH:MM:SSZ). The fourth parameter
varies between the function and defines the scope of the data returned
(Activity or Category).

``` r
library(rescuetimewrapper)
key = "XXX"
start_date = "2021-12-14T00:00:00Z"
end_date = "2021-12-15T23:59:59Z"

# complete
data <- get_rescue_time_data(key,start_date,end_date, 'Activity')
# anonymized
data_anomized <- get_rescue_time_data_anonymized(key,start_date,end_date,"Category")
```

The data returned is a table looking like this

``` r
data <- get_rescue_time_data("XYZ","2021-04-11T00:00:00Z","2021-04-12T23:59:59Z", 'activity')
#> Date                `Time Spent (seconds)` `Number of People` Activity             Category                           Productivity
#> 1 2021-04-11 00:00:00                   1451                  1 idea64               Editing & IDEs                                2
#> 2 2021-04-11 00:00:00                    924                  1 Windows Explorer     General Utilities                             1
#> 3 2021-04-11 00:00:00                    892                  1 discord              General Communication & Scheduling            0
#> 4 2021-04-11 00:00:00                    620                  1 brave                Browsers                                      0
#> 5 2021-04-11 00:00:00                    574                  1 iTunes               Music                                        -2
#> 6 2021-04-11 00:00:00                    495                  1 youtube.com          Video                                        -2

data_anomized <- get_rescue_time_data_anonymized("XYZ","2021-04-12T00:00:00Z","2021-04-12T23:59:59Z", 'Category')
#>                      Category       Date Time Number of Applications
#> 1                    Business 2021-04-12  961                      4
#> 2  Communication & Scheduling 2021-04-12  406                      2
#> 3           Social Networking 2021-04-12   53                      1
#> 4        Design & Composition 2021-04-12   NA                      0
#> 5               Entertainment 2021-04-12 3157                      6
#> 6              News & Opinion 2021-04-12   29                      2
#> 7        Reference & Learning 2021-04-12  301                      5
#> 8        Software Development 2021-04-12 7208                      8
#> 9                    Shopping 2021-04-12   60                      2
#> 10                  Utilities 2021-04-12 1489                      9
#> 11              Uncategorized 2021-04-12  344                      8
```
