Tidyverse contains many subparts such as ggplot2(for plots), tidyr(for data cleaning and tiding), dplyr(for data managements), stringr(for strings), tibble, forcats(for factors) etc. Most of them uses pipe operator which eases workflow for many people. 

- [pipes](#pipes)
- [tidyr Functions](#tidyr-functions)
  * [pivot_longer(): make a dataset longer/in long format:](#pivot-longer-make-a-dataset-longer-in-long-format)
      - [String data in column names:](#string-data-in-column-names)
      - [Numeric data in column names:](#numeric-data-in-column-names)
  * [pivot_wider(): make a dataset wideer/in wide format:](#pivot-wider-make-a-dataset-wideer-in-wide-format)
      - [Simple Example](#simple-example)
      - [Join names](#join-names)
  * [Complex scenarios, when we need both, after one another:](#complex-scenarios-when-we-need-both-after-one-another)
- [dplyr Functions](#functions)
  * [filter(): Return rows with matching conditions](#filter-return-rows-with-matching-conditions)
  * [mutate():  Create or transform variables](#mutate-create-or-transform-variables)
  * [relocate(): Change column order](#relocate-change-column-order)
  * [select(): Select variables by name](#select-select-variables-by-name)
  * [summarise(): Reduce multiple values down to a single value](#summarise-reduce-multiple-values-down-to-a-single-value)


## pipes
In pipe, `x %>% f(y)` basically acts as `f(x, y)`. Often times `x` is an input/data frame, `f` is some operation, `y` is some option to evaluate/alter/filter something from `x`. Usually, if you have to perform a series of action to one data to get some final output, you can do that easily using this pipe. 
For example, 
```R
x %>%
  f_1(y) %>% 
  f_2(z1) %>% 
  f_3(z2)
```
is equivalent to the code in base R
```R
tmp1 <- f_1(x, y)
tmp2 <- f_2(tmp1, z1)
f_3(tmp2, z2)         ## final answer
```

---
## tidyr Functions
We will mostly talk about pivoting a dataframe from 'long' to 'wide' version and vice versa. A similar action can be done using [reshape2]() package also using the `melt()` funtion for wide-to-long and `gather()` function for long-to-wide cases. See [this](http://remi-daigle.github.io/tidyr_reshape2_lesson/) and [this](https://seananderson.ca/2013/10/19/reshape/) link.

This describes the use of the new `pivot_longer()` and `pivot_wider()` functions. Their goal is to improve the usability of `gather()` and `spread()`, and incorporate state-of-the-art features found in other packages. Older funtions are demonstrated [here](http://www.cookbook-r.com/Manipulating_data/Converting_data_between_wide_and_long_format/). 

### pivot_longer(): make a dataset longer/in long format:

##### String data in column names:
It is commonly needed to tidy wild-caught datasets as they often optimise for ease of data entry or ease of comparison rather than ease of analysis. 

```R
relig_income
#> # A tibble: 18 x 11
#>    religion `<$10k` `$10-20k` `$20-30k` `$30-40k` `$40-50k` `$50-75k` `$75-100k`
#>    <chr>      <dbl>     <dbl>     <dbl>     <dbl>     <dbl>     <dbl>      <dbl>
#>  1 Agnostic      27        34        60        81        76       137        122
#>  2 Atheist       12        27        37        52        35        70         73
#>  3 Buddhist      27        21        30        34        33        58         62
#> # … with 15 more rows, and 3 more variables: $100-150k <dbl>, >150k <dbl>,
#> #   Don't know/refused <dbl>
```
This dataset contains three variables:
- religion, stored in the rows,
- income spread across the column names, and
- count stored in the cell values.

```R
relig_income %>% 
  pivot_longer(!religion, names_to = "income", values_to = "count")
#> # A tibble: 180 x 3
#>    religion income             count
#>    <chr>    <chr>              <dbl>
#>  1 Agnostic <$10k                 27
#>  2 Agnostic $10-20k               34
#>  3 Agnostic $20-30k               60
#>  4 Agnostic $30-40k               81
#>  5 Agnostic $40-50k               76
#>  6 Agnostic $50-75k              137
#>  7 Agnostic $75-100k             122
#>  8 Agnostic $100-150k            109
#>  9 Agnostic >150k                 84
#> 10 Agnostic Don't know/refused    96
#> # … with 170 more rows
```
 - The first argument is the dataset to reshape, relig_income.
 - The second argument describes which columns need to be reshaped. In this case, it’s every column apart from religion.
 - The names_to gives the name of the variable that will be created from the data stored in the column names, i.e. income.
 - The values_to gives the name of the variable that will be created from the data stored in the cell value, i.e. count.

Neither the names_to nor the values_to column exists in relig_income, so we provide them as character strings surrounded in quotes.


##### Numeric data in column names:
Look at this kind of data
```R
billboard
#> # A tibble: 317 x 79
#>    artist   track   date.entered   wk1   wk2   wk3   wk4   wk5   wk6   wk7   wk8
#>    <chr>    <chr>   <date>       <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1 2 Pac    Baby D… 2000-02-26      87    82    72    77    87    94    99    NA
#>  2 2Ge+her  The Ha… 2000-09-02      91    87    92    NA    NA    NA    NA    NA
```
We can start with the same basic specification as for the relig_income dataset. Here we want the names to become a variable called week, and the values to become a variable called rank. I also use values_drop_na to drop rows that correspond to missing values. Not every song stays in the charts for all 76 weeks, so the structure of the input data force the creation of unnessary explicit NAs.

```R
billboard %>% 
  pivot_longer(
    cols = starts_with("wk"), 
    names_to = "week", 
    values_to = "rank",
    values_drop_na = TRUE
  )
#> # A tibble: 5,307 x 5
#>    artist  track                   date.entered week   rank
#>    <chr>   <chr>                   <date>       <chr> <dbl>
#>  1 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk1      87
#>  2 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk2      82
#>  3 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk3      72
#>  4 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk4      77
#>  5 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk5      87
#>  6 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk6      94
#>  7 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk7      99
#>  8 2Ge+her The Hardest Part Of ... 2000-09-02   wk1      91
#>  9 2Ge+her The Hardest Part Of ... 2000-09-02   wk2      87
#> 10 2Ge+her The Hardest Part Of ... 2000-09-02   wk3      92
#> # … with 5,297 more rows
```


Complex scenarios will be included later. 

### pivot_wider(): make a dataset wideer/in wide format:
It’s relatively rare to need pivot_wider() to make tidy data, but it’s often useful for creating summary tables for presentation, or data in a format needed by other tools.

##### Simple example
```R
fish_encounters
#> # A tibble: 114 x 3
#>    fish  station  seen
#>    <fct> <fct>   <int>
#>  1 4842  Release     1
#>  2 4842  I80_1       1
#>  3 4842  Lisbon      1
#>  4 4842  Rstr        1
#>  5 4842  Base_TD     1
#>  6 4842  BCE         1
#>  7 4842  BCW         1
#>  8 4842  BCE2        1
#>  9 4842  BCW2        1
#> 10 4842  MAE         1
#> # … with 104 more rows
```

Many tools used to analyse this data need it in a form where each station is a column:

```R
fish_encounters %>% 
  pivot_wider(names_from = station, values_from = seen)
#> # A tibble: 19 x 12
#>    fish  Release I80_1 Lisbon  Rstr Base_TD   BCE   BCW  BCE2  BCW2   MAE   MAW
#>    <fct>   <int> <int>  <int> <int>   <int> <int> <int> <int> <int> <int> <int>
#>  1 4842        1     1      1     1       1     1     1     1     1     1     1
#>  2 4843        1     1      1     1       1     1     1     1     1     1     1
#>  3 4844        1     1      1     1       1     1     1     1     1     1     1
#>  4 4845        1     1      1     1       1    NA    NA    NA    NA    NA    NA
#>  5 4847        1     1      1    NA      NA    NA    NA    NA    NA    NA    NA
#>  6 4848        1     1      1     1      NA    NA    NA    NA    NA    NA    NA
#>  7 4849        1     1     NA    NA      NA    NA    NA    NA    NA    NA    NA
#>  8 4850        1     1     NA     1       1     1     1    NA    NA    NA    NA
#>  9 4851        1     1     NA    NA      NA    NA    NA    NA    NA    NA    NA
#> 10 4854        1     1     NA    NA      NA    NA    NA    NA    NA    NA    NA
#> # … with 9 more rows
```
If we need to replace the `NA`'s by 0, we may add `values_fill = 0` to  `pivot_wider()`. 

##### Join names
Now suppose, we may have to generate column name jointly from many columns name. Suppose we have a data frame like this:
```R
#> # A tibble: 45 x 4
#>    product country  year production
#>    <chr>   <chr>   <int>      <dbl>
#>  1 A       AI       2000     -0.957
#>  2 A       AI       2001      0.444
#>  3 A       AI       2002     -0.535
#>  4 A       AI       2003      1.19 
#>  5 A       AI       2004     -1.55 
#>  6 A       AI       2005      0.702
#>  7 A       AI       2006     -0.812
#>  8 A       AI       2007      0.937
#>  9 A       AI       2008      1.12 
#> 10 A       AI       2009      0.459
#> # … with 35 more rows
```

The following code joins the names to unified column names:
```R
production %>% pivot_wider(
  names_from = c(product, country), 
  values_from = production,
  names_sep = ".",
  names_prefix = "prod."
)
#> # A tibble: 15 x 4
#>     year prod.A.AI prod.B.AI prod.B.EI
#>    <int>     <dbl>     <dbl>     <dbl>
#>  1  2000    -0.957   -1.08       0.277
#>  2  2001     0.444   -0.924      0.468
#>  3  2002    -0.535   -0.404      1.22 
#>  4  2003     1.19     0.677     -1.73 
#>  5  2004    -1.55    -1.45       0.893
#>  6  2005     0.702   -0.0953    -0.792
#>  7  2006    -0.812   -0.186      0.829
#>  8  2007     0.937    0.690     -0.409
#>  9  2008     1.12     1.30      -0.530
#> 10  2009     0.459   -0.880      0.193
#> # … with 5 more rows

production %>% pivot_wider(
  names_from = c(product, country), 
  values_from = production,
  names_glue = "prod_{product}_{country}"
)
#> # A tibble: 15 x 4
#>     year prod_A_AI prod_B_AI prod_B_EI
#>    <int>     <dbl>     <dbl>     <dbl>
#>  1  2000    -0.957   -1.08       0.277
#>  2  2001     0.444   -0.924      0.468
#>  3  2002    -0.535   -0.404      1.22 
#>  4  2003     1.19     0.677     -1.73 
#>  5  2004    -1.55    -1.45       0.893
#>  6  2005     0.702   -0.0953    -0.792
#>  7  2006    -0.812   -0.186      0.829
#>  8  2007     0.937    0.690     -0.409
#>  9  2008     1.12     1.30      -0.530
#> 10  2009     0.459   -0.880      0.193
#> # … with 5 more rows
```

### Complex scenarios, when we need both, after one another:


---
## dplyr Functions

Simply speaking, `dplyr` is a data manipulation paradigm in R which is a part of the `tidyverse`. It accepts the `pipe` operator in R. 
The code dplyr verbs input and output data frames. This contrasts with base R functions which more frequently work with individual vectors.

All dplyr verbs handle “grouped” data frames so that the code to perform a computation per-group looks very similar to code that works on a whole data frame. In base R, per-group operations tend to have varied forms.


### filter(): Return rows with matching conditions

```R
starwars %>% filter(eye_color == "black" & mass > 10)
```
This is closest to `subset` in base R. 

### mutate(): Create or transform variables
`dplyr::mutate()` creates new variables from existing variables:

```R
df %>% mutate(z = x + y, z2 = z ^ 2)
```

When applied to a grouped data frame, dplyr::mutate() computes new variable once per group:
```R
gf <- tibble(g = c(1, 1, 2, 2), x = c(0.5, 1.5, 2.5, 3.5))
gf %>% 
  group_by(g) %>% 
  mutate(x_mean = mean(x), x_rank = rank(x))
#> # A tibble: 4 x 4
#> # Groups:   g [2]
#>       g     x x_mean x_rank
#>   <dbl> <dbl>  <dbl>  <dbl>
#> 1     1   0.5      1      1
#> 2     1   1.5      1      2
#> 3     2   2.5      3      1
#> 4     2   3.5      3      2
```

### relocate(): Change column order

```R
# to front
mtcars %>% relocate(gear, carb) 

# to back
mtcars %>% relocate(mpg, cyl, .after = last_col()) 
```

### select(): Select variables by name

```R
iris %>% select(starts_with("Petal"))
```

with base R, it is 
```R
iris[grep("^Petal", names(iris))]
```


### summarise(): Reduce multiple values down to a single value

`dplyr::summarise()` computes one or more summaries for each group:
```R
mtcars %>% 
  group_by(cyl) %>% 
  summarise(mean = mean(disp), n = n())
#> # A tibble: 3 x 3
#>     cyl  mean     n
#>   <dbl> <dbl> <int>
#> 1     4  105.    11
#> 2     6  183.     7
#> 3     8  353.    14
```

