# Customizing the R startup process

When should I care?

* if you have standard packages that you would always load
* if you want to define custom functions and always have them at hand after the R startup
* if you want to auto-set global options like the number of displayed digits
* if there are project specific options you want to auto-set

## Other sources

* [*Official R help*](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Startup.html)
* Overview over
    * [options to set](https://stat.ethz.ch/R-manual/R-devel/library/base/html/options.html)
    * [environment variables to set](https://stat.ethz.ch/R-manual/R-devel/library/base/html/EnvVar.html)
* [*A great source on the topic. Better than my text!*](https://csgillespie.github.io/efficientR/3-3-r-startup.html)
* [*great examples*](http://www.onthelambda.com/2014/09/17/fun-with-rprofile-and-customizing-r-startup/)

## Basics

* For an overview over the process see [this flowchart](R_startup.pdf).
* For an example ```.Rprofile``` see [this file](.Rprofile)
* [Example explained](#examples)

The R startup process is quite complex, but follows a strait forward logic. It has 3 parts:

1. setting of ```environment variables```
1. sourcing R code in ```profile``` files
1. executing special functions

For number 1. and 2. a distinction is made between ```site``` settings and ```user``` settings. R always checks the ```site``` settings first and the ```user``` settings second. It always uses only one ```profile``` file. Later ones override earlier ones.

The complexity stems from the fact that R tries to be consistent in its startup process over *\*nix* and *Windows* systems. It uses a rather *Unix* oriented approach, but implements that on Windows as well. R's help describes the startup process [in detail](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Startup.html). However, a deeper understanding of the process seems only necessary for multi-user systems. For single user applications you can simply follow the instructions below.

## Notes

* The environment variables which are set by ```.Renviron``` could also be set manually and persistently. Using the ```.Renviron``` approach ensures that they are only set as long as R is running.
* You don't have to set the ```environment variables``` in order to use the ```profile``` files. They only override defaults.

# Auto-loading packages and setting options

The two most common use cases will be *package auto-loading* / *option auto-setting* for a) a single user and for b) a specific project, perhapt overriding the user settings. All of these actions are performed in ```profile``` files. They are R code which is sourced either in the ```base package``` or loaded into the worskpace. You can put these files wherever you want, but you have to point R where. Whether the actions are user specific or project specific is determined by *where* you put the files.

## per user<a name="examples"></a>

For every startup, we want to load the packages ```data.table``` and ```tidyverse```.

1. ```file.edit(file.path("~", ".Rprofile")) # edit .Rprofile in HOME```
    * On Windows, ```Home``` is ```C:\Users\username```. Of course you could re-set this by re-defining the environment variable ```Home```.
1. put in this file ```library(tidyverse); library(data.table)```
1. save the file
1. done

Now when you start R, these packages are loaded. You will see a number of messages. They can be important. Here is how to get rid of the messages. (I stole this from [here](http://www.onthelambda.com/2014/09/17/fun-with-rprofile-and-customizing-r-startup/)).

    sshhh <- function(a.package){
      suppressWarnings(suppressPackageStartupMessages(
        library(a.package, character.only = TRUE)))
    }

    auto.loads <- c("tidyverse", "data.table")
    
    if (interactive()) {
      invisible(sapply(auto.loads, sshhh))
    }


## per project

You may want to set specific options for a project. What you set in the project file will override the earlier settings. Here you could also source custom ```.R``` files you made to load project files.

1. go to the project directory
1. ```file.edit(".Rprofile")```
1. source("my_script.R")
1. save the file
1. done
