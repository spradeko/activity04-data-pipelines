Activity 4
================
spradeko

## Data and packages

Most of the functions that we will use this semester are from the
`{tidyverse}` (which is a package of many packages - a super package).
While we will focus on `{dplyr}`, it will be nice to be able to produce
graphical displays. Therefore, instead of loading both `{dplyr}` and
`{ggplot2}`, in the code chunk below we load all of the `{tidyverse}`.

``` r
library(tidyverse)
```

There is some other useful items to point out from this code chunk:

1.  It has been named (i.e., `load_packages`). Notice that the name
    doesn’t contain any spaces (I used `_` to separate words). If you
    put spaces in your R code chunk names, you might encounter errors
    later when you Knit your document. This is helpful because if you
    create a plot within a code chunk, the image will have that name!
    Another helpful thing by naming code chunks is that it helps with
    document navigation. In the upper right-hand corner of this `.Rmd`
    file, you can see the document outline
    <img src="README-img/document-outline-icon.png" alt="document outline" width = "20"/>
    icon. Clicking on this provides you with a table of contents type
    view. Another way to see this information by clicking on the “\#
    Data and packages” at the bottom left-hand area of this `.Rmd` pane.
2.  There is a code chunk option of `message = FALSE`…

### Messages

Knit your report with the `message = FALSE` R code chunk option and pay
attention to how this particular code chunk and output for loading the
`{tidyverse}` looks.

Now, switch this option to `message = TRUE` and knit your report again.
Discuss what is different when the `message` option is turned on
(`TRUE`) and turned off (`FALSE`) for the previous code chunk.

**Response**:

Changing it to TRUE displays the extra ‘attaching packages’ info and
FALSE takes it away from the knitted document.

Turn the `message` option off and continue in this activity.

### Data

In this activity, we will explore data on college majors and earnings
from the data behind the FiveThirtyEight story [The Economic Guide To
Picking A College
Major](https://fivethirtyeight.com/features/the-economic-guide-to-picking-a-college-major/).
These data originally come from the American Community Survey (ACS)
2010-2012 Public Use Microdata Series. If you are curious about how the
raw data from the ACS were cleaned and prepared (not a requirement), see
the FiveThirtyEight author’s
[code](https://github.com/fivethirtyeight/data/blob/master/college-majors/college-majors-rscript.R).

As we progress through this activity, remember that there are many
considerations that go into picking a major. Earning potential and
employment prospects are two (important) of these considerations, but
they do not tell the entire story. Keep this in mind as we analyze the
data.

These data are from an external (to R) file - a comma separated values
(`.csv`) file. We will use the `readr::read_csv` function to read in
these data. When you see something in this form, I am referring to the
function as well as what package it is from (i.e., `package::function`).
We will talk more about `::` later in the course.

I try to be consistent with notation so here are some other forms that
you might see in documents:

-   “`{package_name}`”; for example, `{readr}`.
-   “`function_name` function”; for example, `read_csv` function.
-   “*hint*”; for example, *arrange* the variable.
-   “**Specific Area**”; for example, **Environment** tab.

``` r
# Note that the :: method is redundant here
# because we have already loaded the {tidyverse}
# and {readr} is included in this.
# However, I am using this method to make you
# aware of the specific package the read_csv
# function comes from.

college_recent_grads <- readr::read_csv("data/recent-grads.csv")
```

Notice that this dataset is in a folder called `data`. We read it in
with the `read_csv` function, then saved the results as a new data frame
called `college_recent_grads`. You can view the entire dataset by
clicking on the name of the data frame in the **Environment** tab (make
sure you have run both the `load_packages` and `load_data` code chunks).

Explore and describe what the `message` option does in `load_data` code
chunk.

**Response**: When message is FALSE it doesn’t desplay the extra column
specification.

Turn the `message` option off and continue in this activity.

#### Asside

Base R has a function for reading in a csv file as well. Type the
following in your **Console** pane, pressing Enter/Return at the end:

    temp <- read.csv("data/recent-grads.csv")

Pause and look at the lack of output/messaging.

Now type the following in your **Console** pane, pressing Enter/Return
at the end:

    temp

Pause and look at the output. You will probably need to scroll up a bit
to see the beginning of it.

Now type the following in your **Console** pane, pressing Enter/Return
at the end:

    college_recent_grads

Pause and look at the output. Compare this to your `temp` output.

If you were to load the data using `read.csv` in an `.Rmd` file and try
to view it, it will print the ENTIRE dataset - this could make a short
report many hundreds of pages depending on the size of your dataset.
Historically, there were some other benefits of `readr::read_csv` over
`read.csv`, but these have mostly been updated to no longer be issues. I
encourage you to use `readr::read_csv` instead of `read.csv`.

## Quick Data View

Below is a blank R code chunk. Name this code chunk `college_grads_top`;
you do not need to suppress messages. In this code chunk, type
`college_recent_grads` to view the top few (first 10) entries of the
dataset.

``` r
college_recent_grads
```

    ## # A tibble: 173 x 21
    ##     rank major_code major           major_category total sample_size   men women
    ##    <dbl>      <dbl> <chr>           <chr>          <dbl>       <dbl> <dbl> <dbl>
    ##  1     1       2419 Petroleum Engi… Engineering     2339          36  2057   282
    ##  2     2       2416 Mining And Min… Engineering      756           7   679    77
    ##  3     3       2415 Metallurgical … Engineering      856           3   725   131
    ##  4     4       2417 Naval Architec… Engineering     1258          16  1123   135
    ##  5     5       2405 Chemical Engin… Engineering    32260         289 21239 11021
    ##  6     6       2418 Nuclear Engine… Engineering     2573          17  2200   373
    ##  7     7       6202 Actuarial Scie… Business        3777          51  2110  1667
    ##  8     8       5001 Astronomy And … Physical Scie…  1792          10   832   960
    ##  9     9       2414 Mechanical Eng… Engineering    91227        1029 80320 10907
    ## 10    10       2408 Electrical Eng… Engineering    81527         631 65511 16016
    ## # … with 163 more rows, and 13 more variables: sharewomen <dbl>,
    ## #   employed <dbl>, employed_fulltime <dbl>, employed_parttime <dbl>,
    ## #   employed_fulltime_yearround <dbl>, unemployed <dbl>,
    ## #   unemployment_rate <dbl>, p25th <dbl>, median <dbl>, p75th <dbl>,
    ## #   college_jobs <dbl>, non_college_jobs <dbl>, low_wage_jobs <dbl>

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Data Codebook

Descriptions of the variables are provided below.

| Header                         | Description                                                                 |
|:-------------------------------|:----------------------------------------------------------------------------|
| `rank`                         | Rank by median earnings                                                     |
| `major_code`                   | Major code, FO1DP in ACS PUMS                                               |
| `major`                        | Major description                                                           |
| `major_category`               | Category of major from Carnevale et al                                      |
| `total`                        | Total number of people with major                                           |
| `sample_size`                  | Sample size (unweighted) of full-time, year-round ONLY (used for earnings)  |
| `men`                          | Male graduates                                                              |
| `women`                        | Female graduates                                                            |
| `sharewomen`                   | Women as share of total                                                     |
| `employed`                     | Number employed (ESR == 1 or 2)                                             |
| `employed_full_time`           | Employed 35 hours or more                                                   |
| `employed_part_time`           | Employed less than 35 hours                                                 |
| `employed_full_time_yearround` | Employed at least 50 weeks (WKW == 1) and at least 35 hours (WKHP &gt;= 35) |
| `unemployed`                   | Number unemployed (ESR == 3)                                                |
| `unemployment_rate`            | Unemployed / (Unemployed + Employed)                                        |
| `median`                       | Median earnings of full-time, year-round workers                            |
| `p25th`                        | 25th percentile of earnings                                                 |
| `p75th`                        | 75th percentile of earnings                                                 |
| `college_jobs`                 | Number with job requiring a college degree                                  |
| `non_college_jobs`             | Number with job not requiring a college degree                              |
| `low_wage_jobs`                | Number in low-wage service jobs                                             |

We can see that this data frame has a lot of useful information. In the
remaining sections we will answer the following questions:

-   Which major has the lowest unemployment rate?
-   Which major has the highest percentage of women?
-   Which STEM majors have incomes less than the median income of all
    majors?

Note that the ACS only asks [one
question](https://www.census.gov/acs/www/about/why-we-ask-each-question/sex/)
about a person’s sexual identity and we are therefore missing a lot of
important details about individuals.

## Analysis

### Which major has the lowest unemployment rate?

#### Describe your process

What information (variables) do we need to answer this question?
Describe how you would go about answering this question using a
different software (e.g., Excel, SAS, Python) that you are familiar with
or simply a general process. You do not need to code anything. As you
write this process, think of it as a series of steps and it might be
helpful to start at the goal and work backwards.

**Response**: Often, I’d have to use nested functions or take it line by
line as functions and run several separate functions at a time.

#### Using `{dplyr}`

In the R code chunk below, name it `rearrange_college`.

Take the `college_recent_grads` dataset, *then* `arrange` the dataset by
`unemployment_rate`.

``` r
college_recent_grads %>%
arrange(unemployment_rate)
```

    ## # A tibble: 173 x 21
    ##     rank major_code major        major_category    total sample_size   men women
    ##    <dbl>      <dbl> <chr>        <chr>             <dbl>       <dbl> <dbl> <dbl>
    ##  1    53       4005 Mathematics… Computers & Math…   609           7   500   109
    ##  2    74       3801 Military Te… Industrial Arts …   124           4   124     0
    ##  3    84       3602 Botany       Biology & Life S…  1329           9   626   703
    ##  4   113       1106 Soil Science Agriculture & Na…   685           4   476   209
    ##  5   121       2301 Educational… Education           804           5   280   524
    ##  6    15       2409 Engineering… Engineering        4321          30  3526   795
    ##  7    20       3201 Court Repor… Law & Public Pol…  1148          14   877   271
    ##  8   120       2305 Mathematics… Education         14237         123  3872 10365
    ##  9     1       2419 Petroleum E… Engineering        2339          36  2057   282
    ## 10    65       1100 General Agr… Agriculture & Na… 10399         158  6053  4346
    ## # … with 163 more rows, and 13 more variables: sharewomen <dbl>,
    ## #   employed <dbl>, employed_fulltime <dbl>, employed_parttime <dbl>,
    ## #   employed_fulltime_yearround <dbl>, unemployed <dbl>,
    ## #   unemployment_rate <dbl>, p25th <dbl>, median <dbl>, p75th <dbl>,
    ## #   college_jobs <dbl>, non_college_jobs <dbl>, low_wage_jobs <dbl>

We have all of the information to answer our question (i.e., “Which
major has the lowest unemployment rate?”), but it is not in an effective
format. The major names barely fit on the page, we have many variables
are not that useful in answering our question (e.g., `major_code`,
`major_category`), and some variables that we might want
front-and-center are not easily viewed (i.e., `unemployment_rate`).
Also, notice that by default when you *arrange* by a variable, it
defaults to *ascending* order.

#### Removing unnecessary information

Use the pipeline that you made in the `rearrange_college` code chunk as
the start to this solution, **then** `select` only the variables `rank`,
`major`, and `unemployment_rate`. Name this code chunk
`lowest_unemploy`.

``` r
college_recent_grads %>%
arrange(unemployment_rate) %>%
select(rank,major,unemployment_rate)
```

    ## # A tibble: 173 x 3
    ##     rank major                                      unemployment_rate
    ##    <dbl> <chr>                                                  <dbl>
    ##  1    53 Mathematics And Computer Science                     0      
    ##  2    74 Military Technologies                                0      
    ##  3    84 Botany                                               0      
    ##  4   113 Soil Science                                         0      
    ##  5   121 Educational Administration And Supervision           0      
    ##  6    15 Engineering Mechanics Physics And Science            0.00633
    ##  7    20 Court Reporting                                      0.0117 
    ##  8   120 Mathematics Teacher Education                        0.0162 
    ##  9     1 Petroleum Engineering                                0.0184 
    ## 10    65 General Agriculture                                  0.0196 
    ## # … with 163 more rows

**Response**: It is very easy to follow all the steps I have taken to
select out the information that I want.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Which major has the highest proportion of women?

Using the `college_recent_grads` dataset and functions from `{dplyr}`,
`arrange` the dataset by `sharewomen`, and `select` only `rank`,
`major`, and `sharewomen`. Name your code chunk `highest_prop_women`.

``` r
college_recent_grads %>%
  arrange(desc(sharewomen)) %>%
  select(rank,major,sharewomen) 
```

    ## # A tibble: 173 x 3
    ##     rank major                                         sharewomen
    ##    <dbl> <chr>                                              <dbl>
    ##  1   165 Early Childhood Education                          0.969
    ##  2   164 Communication Disorders Sciences And Services      0.968
    ##  3    52 Medical Assisting Services                         0.928
    ##  4   139 Elementary Education                               0.924
    ##  5   151 Family And Consumer Sciences                       0.911
    ##  6   101 Special Needs Education                            0.907
    ##  7   157 Human Services And Community Organization          0.906
    ##  8   152 Social Work                                        0.904
    ##  9    35 Nursing                                            0.896
    ## 10    89 Miscellaneous Health Medical Professions           0.881
    ## # … with 163 more rows

Discuss your output as it relates to the research question.

**Response**: The initial info displayed started at the lowest amount
women in majors, I had to sort by descending to find the highest
concentration.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Which STEM majors have incomes less than the median income of all majors?

One of the sections of the FiveThirtyEight story is “All STEM fields
aren’t the same”. We will see if this is true.

Below is a new vector called `stem_categories` that lists the *major
categories* (this is not the `majors` variable - look back at the Data
Codebook) that are considered STEM fields.

``` r
stem_categories <- c("Biology & Life Science",
                     "Computers & Mathematics",
                     "Engineering",
                     "Physical Sciences")
```

In our next activity we will see how to calculate summary statistics,
but for now believe me that the median salary for all majors is $36,000.

Using the `stem_categories` vector and knowledge that the median salary
for all majors is $36,000 to *keep* the rows in the
`college_recent_grads` data set that are in the STEM fields and earn
*less* than the `median` salary of all majors. Only display the
variables `major`, `p25th`, `median`, and `p75th` Name the code chunk
`stem_low_salaries`.

``` r
college_recent_grads %>%
filter(major_category %in% stem_categories) %>% 
filter(median < 36000) %>% 
select(major,p25th,median,p75th)
```

    ## # A tibble: 10 x 4
    ##    major                                 p25th median p75th
    ##    <chr>                                 <dbl>  <dbl> <dbl>
    ##  1 Environmental Science                 25000  35600 40200
    ##  2 Multi-Disciplinary Or General Science 24000  35000 50000
    ##  3 Physiology                            20000  35000 50000
    ##  4 Communication Technologies            25000  35000 45000
    ##  5 Neuroscience                          30000  35000 44000
    ##  6 Atmospheric Sciences And Meteorology  28000  35000 50000
    ##  7 Miscellaneous Biology                 23000  33500 48000
    ##  8 Biology                               24000  33400 45000
    ##  9 Ecology                               23000  33000 42000
    ## 10 Zoology                               20000  26000 39000

Discuss your output as it relates to the research question.

**Response**: It took a little finagling to get the code organized how I
wanted it but I like how easy it is to read and display with so few
functions!

![](README-img/noun_pause.png) **(Final) Planned Pause Point**: If you
have any questions, contact your instructor. Otherwise feel free to
continue on.

Knit, then stage everything listed in your **Git** pane, commit (with a
meaningful commit message), and push to your GitHub repo. Go to GitHub
and verify that your `activity04-data-pieplines.Rmd` file appears as you
intended it to.

You can now go back to the `README` file.

## Attribution

This activity is inspired by a lab from [Dr. Mine
Çetinkaya-Rundel](http://www2.stat.duke.edu/~mc301/)’s STA 199 course.
