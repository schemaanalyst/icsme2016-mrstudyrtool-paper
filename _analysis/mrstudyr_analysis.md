---
title: "MRSTUDYR: Retrospectively Studying the Effectiveness of Mutant Reduction Techniques"
output:
  pdf_document:
    keep_tex: true
    highlight: pygments
    number_sections: true
---

# Installing the Ineffective Mutant Analysis Package

This code will install the ineffectivemutants package from GitHub using the `install_github` function.


```r
options(scipen=10, width=200)
devtools::install_github("mccurdyc/mrstudyr")
```

```
## Downloading GitHub repo mccurdyc/mrstudyr@master
## from URL https://api.github.com/repos/mccurdyc/mrstudyr/zipball/master
```

```
## Installing mrstudyr
```

```
## '/Library/Frameworks/R.framework/Resources/bin/R' --no-site-file --no-environ --no-save --no-restore --quiet CMD INSTALL  \
##   '/private/var/folders/63/3rz5g1g93kl56014nnctmg6w0000gn/T/Rtmp5To3sr/devtools2afd19c11734/mccurdyc-mrstudyr-44792b8' --library='/Library/Frameworks/R.framework/Versions/3.3/Resources/library'  \
##   --install-tests
```

```
## 
```

> # Comparing the Mutation Scores

## Initialise the System

First, load in the libraries that are used in addition to those with the mrstudyr package (i.e., load all
of the packages not used by mrstudyr but still used in this RMarkdown file). Note that right now the
mrstudyr package will automatically load all of the packages that it needs to performs its various
analyses. Now, we are ready to call the functions from the ineffectivemutants package and produce the appropriate
summary tables and graphs.


```r
suppressPackageStartupMessages(library(mrstudyr))
suppressPackageStartupMessages(library(knitr))
```

## Show the Schemas for the ICSME 2016 Paper


```r
sqlite_data <- read_sqlite_avmdefaults()
```

```
## Parsed with column specification:
## cols(
##   identifier = col_character(),
##   dbms = col_character(),
##   schema = col_character(),
##   operator = col_character(),
##   type = col_character(),
##   killed = col_character(),
##   time = col_integer()
## )
```

```r
schemas <- sqlite_data %>% collect_study_schemas() %>% collect_normal_data() %>% select(schema) %>% unique()
```

```
## Error in function_list[[i]](value): could not find function "collect_normal_data"
```

```r
knitr::kable(schemas, format="latex", booktabs=TRUE)
```

```
## Error in inherits(x, "list"): object 'schemas' not found
```

## Perform Reduction Techniques

First, performing the most common reduction technique, random sampling.


```r
d <- read_sqlite_avmdefaults()
```

```
## Parsed with column specification:
## cols(
##   identifier = col_character(),
##   dbms = col_character(),
##   schema = col_character(),
##   operator = col_character(),
##   type = col_character(),
##   killed = col_character(),
##   time = col_integer()
## )
```

```r
normal_data <- d %>% collect_study_schemas() %>% collect_normal_data()
```

```
## Error in function_list[[k]](value): could not find function "collect_normal_data"
```

```r
random_sampling_data <- d %>% analyse_random_sampling()
```

```
## [1] "RANDOM SAMPLING: Currently analysing x = 1 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 10 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 20 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 30 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 40 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 50 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 60 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 70 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 80 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 90 percent ..."
## [1] "RANDOM SAMPLING: Currently analysing x = 100 percent ..."
```

```r
knitr::kable(head(random_sampling_data), format="latex", booktabs=TRUE)
```


\begin{tabular}{llrrrrrrrrrrr}
\toprule
dbms & schema & reduced\_numerator & reduced\_denominator & original\_numerator & original\_denominator & reduced\_time & original\_time & cost\_reduction & reduced\_mutation\_score & original\_mutation\_score & percentage & trial\\
\midrule
SQLite & ArtistSimilarity & 3 & 4 & 360 & 390 & 37 & 3543 & 0.9895569 & 75.00000 & 92.30769 & 1 & 1\\
SQLite & ArtistTerm & 9 & 9 & 780 & 870 & 156 & 19090 & 0.9918282 & 100.00000 & 89.65517 & 1 & 1\\
SQLite & BankAccount & 11 & 13 & 1080 & 1260 & 172 & 16458 & 0.9895492 & 84.61538 & 85.71429 & 1 & 1\\
SQLite & BookTown & 56 & 70 & 5820 & 7050 & 11289 & 984232 & 0.9885301 & 80.00000 & 82.55319 & 1 & 1\\
SQLite & BrowserCookies & 30 & 34 & 3030 & 3420 & 654 & 62949 & 0.9896106 & 88.23529 & 88.59649 & 1 & 1\\
SQLite & Cloc & 9 & 9 & 900 & 900 & 118 & 11418 & 0.9896654 & 100.00000 & 100.00000 & 1 & 1\\
\bottomrule
\end{tabular}

Next, applying the concepts of random sampling but, over operators.


```r
d <- read_sqlite_avmdefaults()
```

```
## Parsed with column specification:
## cols(
##   identifier = col_character(),
##   dbms = col_character(),
##   schema = col_character(),
##   operator = col_character(),
##   type = col_character(),
##   killed = col_character(),
##   time = col_integer()
## )
```

```r
normal_data <- d %>% collect_study_schemas() %>% collect_normal_data()
```

```
## Error in function_list[[k]](value): could not find function "collect_normal_data"
```

```r
operator_sampling_data <- d %>% analyse_operator_sampling()
```

```
## Error in function_list[[k]](value): could not find function "analyse_operator_sampling"
```

```r
knitr::kable(head(operator_sampling_data), format="latex", booktabs=TRUE)
```

```
## Error in head(operator_sampling_data): object 'operator_sampling_data' not found
```
## Visualise the Mutation Scores for the Random Sampling Technique

Now, plot the graph that shows the mutation score for the random sampling mutant reduction technique for four user-specified percentages.
In this example, we will create a visualisation containing the percentages
found in the accompanying paper.


```r
d <- read_sqlite_avmdefaults()
```

```
## Parsed with column specification:
## cols(
##   identifier = col_character(),
##   dbms = col_character(),
##   schema = col_character(),
##   operator = col_character(),
##   type = col_character(),
##   killed = col_character(),
##   time = col_integer()
## )
```

```r
normal_data <- d %>% collect_study_schemas() %>% collect_normal_data()
```

```
## Error in function_list[[k]](value): could not find function "collect_normal_data"
```

```r
filtered_percents_data <- random_sampling_data %>% collect_chosen_percent_data(c(1, 10, 20, 40))
visualise_random_sampling_mutation_scores(filtered_percents_data)
```

```
## Error in grid.newpage(): cairo error 'error while writing to output stream'
```

Now, plot the graph that shows the mutation score for the operator sampling mutant reduction technique.


```r
d <- read_sqlite_avmdefaults()
```

```
## Parsed with column specification:
## cols(
##   identifier = col_character(),
##   dbms = col_character(),
##   schema = col_character(),
##   operator = col_character(),
##   type = col_character(),
##   killed = col_character(),
##   time = col_integer()
## )
```

```r
normal_data <- d %>% collect_study_schemas() %>% collect_normal_data()
```

```
## Error in function_list[[k]](value): could not find function "collect_normal_data"
```

```r
filtered_percents_data <- operator_sampling_data %>% collect_chosen_percent_data(c(1, 10, 20, 40))
```

```
## Error in eval(expr, envir, enclos): object 'operator_sampling_data' not found
```

```r
visualise_operator_sampling_mutation_scores(filtered_percents_data)
```

```
## Error in grid.newpage(): cairo error 'error while writing to output stream'
```
Analysis of the results in this graph:

- Much more variability for the time-constrained method than the virtual one
- However, the mutation scores are often roughly similar between the two techniques, especially for the HyperSQL and
  SQLite DBMSs
- It is important to note that the time-constrained method yields mutation scores that are higher than those produced by
  the virtual mutation technique (e.g., CoffeeOrders and Employee schemas). And, note that the virtual method
  leads to a score that is the same as the standard one. So, time constrained will over estimate the scores sometimes.
- Greater variability in the Person schema is due to the fact that sometimes the mutation score is zero (i.e., all of
  the mutants are killed) and other times it is one (i.e., none of the mutants are killed)

Additional data points to support this analysis:


```r
time_constrained_data_tidy_print <- time_constrained_data_tidy %>%
  filter(schema %in% c("Person")) %>%
  filter(dbms %in% c("PostgreSQL")) %>%
  filter(score_type %in% c("Time-Constrained"))
```

```
## Error in eval(expr, envir, enclos): object 'time_constrained_data_tidy' not found
```

```r
knitr::kable(time_constrained_data_tidy_print, format="latex", booktabs=TRUE)
```

```
## Error in inherits(x, "list"): object 'time_constrained_data_tidy_print' not found
```
Questions about the results analysis:

- Is there every a case where we want to do statistical analysis or effect size calculations?
- I think that the "transformation" idea from Harman would apply to our analysis of the timing values.
