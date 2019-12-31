# Data Wrangling with R

Code and text for the 2nd edition of the [_Data Wrangling with R_](https://www.springer.com/us/book/9783319455983) book.
Please help me make it better by [contributing](contributing.md)!

## Installing dependencies

Install the R packages used by the book with:

```r
# install.packages("devtools")
devtools::install_deps()
```

## Build the book

In RStudio, press Cmd/Ctrl + Shift + B. Or run:

```r
# HTML version
bookdown::render_book("index.Rmd")

# PDF version
bookdown::render_book("index.Rmd", "bookdown::pdf_book")
```