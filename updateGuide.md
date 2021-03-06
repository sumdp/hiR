##10 STEP GUIDE TO CREATING/UPDATING A R PACKAGE##

[h/t @ hadley](http://scholarship.rice.edu/bitstream/handle/1911/36084/r-packages.key.pdf?sequence=2)

If you'd like to add a function to this or any R package,
just follow these 10 simple steps:

OPTIONAL: If you want to create a new package from scratch, start here:

    #R
    p <- "rpckg"
    library("devtools")
    create(p)

1. Or just clone a repository from github

    ```
    #!/bin/bash
    git clone git://github.com/hinstitute/rpckg.git
    ```

    Alternatively, install directly with ``devtools::install_github``

    ```
    #R
    library("devtools")
    install_github(p, "hinstitute")
    library(p)
    ```

2. Carefully format new functions you want to add according to this template: ([more info](https://github.com/hadley/devtools/wiki/docs-function))

    ```
    #' A short description of the R function
    #'
    #' A more detailed description of the function
    #'
    #' @param x A description of parameter x's requirements
    #' @param y A description of parameter y's requirements
    #'
    #' @return
    #' An explanation of the values the function returns
    #'
    #' @export
    #'
    #' @example
    #' # a simple example
    #' x <- 2
    #' y <- 2
    #' library("rpckg")
    #' add(x, y)
    # insert your function below
    add <- function(x, y) {
        x + y
    }
    ```

3. Save each of your functions in the ``R`` subdirectory with a ``.R`` file extension, e.g. to ``rpckg/R/add.R``

4. Run the following commands in the pacakge's parent directory to build your ``NAMESPACE`` and ``DESCRIPTION`` files, and your ``man`` directory of ``.Rd`` files.

    ```
    #R
    library("roxygen2")
    roxygenize(p)
    ```

5. Open the ``DESCRIPTION`` file and make sure it follows this template,
   be careful to include all necessary packages under ``Depends:``.

    ```
    #DESCRIPTION
    Package: rpckg
    Title: A package that does someting in R
    Description: Explanation of what the package does broadly
    Version: 0.1
    Author: name1 <name1@domain.come>
    Maintainers: name2 <name1@domain.come>
    License: MIT
    Depends:
        R (>= 2.15.1)
        # your required packages would go here, separated by commas every line
    Suggests:
        # your suggested packages would go here in the same format.
        # these are packages the library rarely uses or only uses for examples.
    LazyLoad: yes
    Collate:
        'add.R' #always use '' and no commas, only filepath per line
    ```

6. Make sure the functions in your ``NAMESAKE`` file match the functions under ``Collate:`` within your ``DESCRPTION`` file, e.g:

    ```
    #NAMESAKE
    export(add.R)
    ```

7. Navigate to ``rpckg``'s parent directory and run this in the command line. Make note of any error messages.

    ```
    #!/bin/bash
    R CMD check rpckg
    ```

    OPTIONAL: build the package and push to ``cran``

    ```
    #!/bin/bash
    R CMD build rpckg
    ftp -u ftp://cran.r-projects.org/incoming/ rpckg_0.1.tar.gz
    ```

8. OR, Locate the ``rpckg.Rcheck`` folder in the package's parent directory,
   open this folder, move ``rpckg.pdf`` and ``rpckg-ex.pdf`` into ``rpckg`` or ``rpckg/docs``
   and overwite if they already exist.

9. Now push these updates to git, e.g:

    ```
    #!/bin/bash
    cd rpckg
    cd git add .
    git commit -m "my first commit"
    git push
    ```

10. Finally, (re)install the package using ``devtools::install_github``:

    ```
    #R
    library("devtools")
    install_github(p, "hinstitute")
    library(p)
    help(package=p)
    ```

_WARNING: if you install the package twice in the same R Session the manual will break. Simply restart R to fix this issue_

