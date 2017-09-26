# Customizing the R startup process

* [*A great source on the topic. Better than my text!*](https://csgillespie.github.io/efficientR/3-3-r-startup.html)
* [*cool examples*](http://www.onthelambda.com/2014/09/17/fun-with-rprofile-and-customizing-r-startup/)

The R startup process is quite complex, but follows a strait forward logic. 
* For an overview over the process see [this flowchart](flowchart.pdf).
* [Examples](#examples)

There are 3 parts:

1. setting of ```environment variables```
1. sourcing R code in ```profile``` files
1. executing special functions

For number 1. and 2. a distinction is made between ```site``` settings and ```user``` settings. R always checks the ```site``` settings first and the ```user``` settings second. From the documentation it is not entirely clear which override which, but one would assume that those called later (the ```user``` settings) override those called first.

The complexity probably stems from the fact that R tries to be consistent in its startup process over *\*nix* and *Windows* systems. It uses a rather *Unix* oriented approach, but implements that on Windows as well. R's help describes the startup process [in detail](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Startup.html), but is not always entirely clear on the process. This would have to be determined from the source code or by trial-and-error. However, a deeper understanding of the process seems only necessary for multi-user systems. For single user applications you can simply follow the instructions below.

*Notes*

* The environment variables which are set by ```.Renviron``` could also be set manually and persistently. Using the ```.Renviron``` approach ensures that they are only set as long as R is running.
* You don't have to set the ```environment variables``` in order to use the ```profile``` files. They only override defaults.

# When should I care?

* if you have standard packages that you would always load
* if you want to define custom functions and always have them at hand after the R startup
* if you want to auto-set global options like the number of displayed digits
* if there are project specific options you want to auto-set

# Auto-loading packages and setting options

The two most common use cases will be *package auto-loading* / *option auto-setting* for a) a single user and for b) a specific project, perhapt overriding the user settings. All of these actions are performed in ```profile``` files. They are R code which is sourced either in the ```base package``` or loaded into the worskpace. You can put these files wherever you want, but you have to point R where. Whether the actions are user specific or project specific is determined by *where* you put the files.

## per user<a name="examples"></a>

For every startup, we want to load the packages ```data.table``` and ```tidyverse```.

1. ```file.edit(file.path("~", ".Rprofile")) # edit .Rprofile in HOME```
    * On Windows, ```Home``` is ```C:\Users\username```. Of course you could re-set this by re-defining the environment variable ```Home```.
1. ```library(tidyverse); library(data.table)```

"I don't want to see the messages during startup."

    sshhh <- function(a.package){
      suppressWarnings(suppressPackageStartupMessages(
        library(a.package, character.only = TRUE)))
    }

    auto.loads <- c("tidyverse", "data.table")
    
    if (interactive()) {
      invisible(sapply(auto.loads, sshhh))
    }


## per project

"I want to load my custom ```.R``` file during project startup."

1. go to the project directory
1. ```file.edit(".Rprofile")```
1. source("my_script.R")
