local({r <- getOption("repos")
r["CRAN"] <- "https://ftp.gwdg.de/pub/misc/cran/"
options(repos = r)})

options(digits.secs = 3,
        digits = 15,         # max digits to display
        dplyr.width = Inf,
        editor = "vim",
        max.print = 100,    # max items to print
        scipen = 10,        # something with number display
        width = 80
)


# (quiet) Auto loads
sshhh <- function(a.package){
  suppressWarnings(suppressPackageStartupMessages(
    library(a.package, character.only = TRUE)))
}

auto.loads <- c("tidyverse", "data.table")

if (interactive()) {
  invisible(sapply(auto.loads, sshhh))
}

# custom functions
.env <- new.env()

## get mode of a vector
.env$Mode <- function(x) {

  stopifnot(is.vector(x))

  rles <- rle(x)
  out <- rles$values[which(max(rles$lengths) == rles$lengths)]

  out

}

## get rid of rownames
.env$unrowname <- function(x) {

  rownames(x) <- NULL
  x
}

## factor to character
.env$unfactor <- function(df){
  id <- sapply(df, is.factor)
  df[id] <- lapply(df[id], as.character)
  df
}

attach(.env)

# finally
message("\n*** Successfully loaded .Rprofile ***
         \n*** Working Directory is", getwd(), "***\n\n")

