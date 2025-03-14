
<!-- README.md is generated from README.Rmd. Please edit that file -->

# `ClinTrialX`

<!-- badges: start -->

[![R-CMD-check](https://github.com/ineelhere/clintrialx/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/ineelhere/clintrialx/actions/workflows/R-CMD-check.yaml)
[![License:
Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](https://opensource.org/license/apache-2-0)
[![pkgdown](https://img.shields.io/badge/pkgdown-docs-blue.svg)](https://www.indraneelchakraborty.com/clintrialx/)
[![Visitors](https://api.visitorbadge.io/api/visitors?path=https%3A%2F%2Fgithub.com%2Fineelhere%2Fclintrialx&label=Visitors&labelColor=%23f47373&countColor=%2337d67a&style=flat&labelStyle=upper)](https://github.com/ineelhere/clintrialx)
[![CRAN
status](https://www.r-pkg.org/badges/version/clintrialx)](https://CRAN.R-project.org/package=clintrialx)
[![CRAN
downloads](https://cranlogs.r-pkg.org/badges/clintrialx)](https://CRAN.R-project.org/package=clintrialx)

[![Data Sources:
ClinicalTrials.gov](https://img.shields.io/badge/Data_Sources-ClinicalTrials.gov-blue)](https://clinicaltrials.gov/data-api/api)
[![Data Sources: CTTI
AACT](https://img.shields.io/badge/Data_Sources-CTTI%20AACT%20-purple)](https://aact.ctti-clinicaltrials.org/)

<!-- badges: end -->

The goal of `{clintrialx}` is to fetch clinical trials data from freely
available registries. Currently, it supports querying the

- [ClinicalTrials.gov](https://clinicaltrials.gov/) registry using its
  [V2 API](https://clinicaltrials.gov/data-api/api) and

- [CTTI AACT](https://aact.ctti-clinicaltrials.org/) (Public Access to
  Aggregate Content of ClinicalTrials.gov).

## Installation

Install the package from
[CRAN](https://CRAN.R-project.org/package=clintrialx) with:

``` r
install.packages("clintrialx")
```

You can install this package from
[GitHub](https://github.com/ineelhere/clintrialx) with:

*you’ll need the `devtools` package for this*

``` r
# install.packages("devtools")
devtools::install_github("ineelhere/clintrialx")
```

### Check installation

``` r
library(clintrialx)
```

## Setup AACT account

#### `Only if you wish to use AACT as a source for the data`

- Visit <https://aact.ctti-clinicaltrials.org/users/sign_up>

- Sign up and create an account. It’s free.

- The `username` and `password` will be needed to fetch data using this
  package.

- Save it in a `.Renviron` file, for example-

  ``` r
  user =  "random_name"
  password = "random_password"
  ```

- Now that the file is created, load the variable with the command
  `readRenviron("path/to/.Renviron)`

- You’re all set!

## Query the [ClinicalTrials.gov](https://clinicaltrials.gov/) Registry

#### Based on NCT IDs

Fetch one or multiple trial records based on NCT IDs. You can opt to
fetch some specific fields or all fields available at source (default).

``` r
library(clintrialx)
ctg_get_nct(c("NCT02967965", "NCT04000165", "NCT01007279", "NCT02376244", "NCT01179776"),
                          fields = c("NCT Number", "Study Title", "Study Status", "Sponsor"))
```

#### Based on fileds

Supports filtering by condition, location, title keywords, intervention,
and overall status.

``` r
ctg_get_fields(
     condition = "Cancer",
     location = "Kolkata",
     title = NULL,
     intervention = "Drug",
     status = c("ACTIVE_NOT_RECRUITING", "RECRUITING"),
     page_size = 10
)
```

#### Based on fields - Bulk download

Download all available data for your query. No limits!

*Supports filtering by condition, location, title keywords,
intervention, and overall status.*

``` r
df <- ctg_bulk_fetch(location="india")
```

## Query the [CTTI AACT](https://aact.ctti-clinicaltrials.org/)

#### Run Custom Queries

    # Set environment variables for database credentials in .Renviron and load it
    # readRenviron(".Renviron")

    # Connect to the database
    con <- aact_connection(Sys.getenv('user'), Sys.getenv('password'))

    # Run a custom query
    query <- "SELECT nct_id, source, enrollment, overall_status FROM studies LIMIT 5;"
    results <- aact_custom_query(con, query)

    # Print the results
    print(results)

## Trial Data HTML Reports

*Currently works for data from ClinicalTrials.Gov*

Visit here for an exqample report -
<https://www.indraneelchakraborty.com/clintrialx/report.html>

``` r
#first get the data in a R dataframe
my_clinical_trial_data <- ctg_bulk_fetch(condition="cancer")
#now pass it to the reports function
ctg_data_report(
  ctg_data = my_clinical_trial_data,
  title = "Clinical Trials Analysis",
  author = "Indra",
  output_file = "reports/clinical_trials.html",
  theme = "flatly",
  color_palette = c("#4E79A7", "#F28E2B", "#E15759", "#76B7B2", "#59A14F", "#EDC948"),
  include_data_quality = TRUE,
  include_interactive_plots = TRUE,
  custom_footer = "Proprietary report generated by SomeGreatOrg Inc."
)

# Generate a report with static plots and no data quality assessment
ctg_data_report(
  ctg_data = my_clinical_trial_data,
  title = "Quick Clinical Trial Overview",
  include_data_quality = FALSE,
  include_interactive_plots = FALSE
)
```

Check the path `reports/clinical_trials.html` on your local. It will
have the html report file.

Cool stuff - It also has the codes to the plots!

## Data Sources

You can fetch version information directly from the package:

``` r
version_info(source = "clinicaltrials.gov")
```

## Get Involved

🚀 Ready to contribute? Let’s make clintrialx even better!

- Fork the [GitHub](https://github.com/ineelhere/clintrialx) repo.
- Check out your development branch from the dev branch.
- Do your work on a feature (ftr) branch.
- Raise a PR against the dev branch of the
  [clintrialx](https://github.com/ineelhere/clintrialx) repo.
- Sit back and [relax](https://www.youtube.com/watch?v=Uffjii1hXzU)
  while I review it!

💬 **Questions or Feedback?** Feel free to open an issue on [GitHub
Issues page](https://github.com/ineelhere/clintrialx/issues).

🌟 **Enjoying `clintrialx`?** Please consider giving it a star on
[GitHub](https://github.com/ineelhere/clintrialx)! Your support helps
this project grow and improve.

More updates to come. Happy coding! 🎉
