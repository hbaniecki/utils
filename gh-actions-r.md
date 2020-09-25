# gh-actions

Configure GitHub Actions for R

- [push-pull configuration](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- https://github.com/r-lib/usethis
- https://github.com/maxheld83/ghactions
- https://github.com/r-lib/pkgdown
- https://github.com/r-lib/remotes
- https://github.com/r-lib/actions

## Tutorial for R Configuration (v0.1.0)

0. make a coffee

1. put additional *.png/.gif* files used e.g. in `README.md` into the `man/figures` folder
- remove `man/figures` from `.rbuildignore` 
- add to `.rbuildignore`:

```
^\.github$
^.*\.gif
^.*\.png
^pkgdown$
```

- fix path in `README.md` `images/*.png -> man/figures/*.png`

2. if hosting any additional *\*.html* files like this: *website.com/\*.html* put them into the `pkgdown/favicon` folder

3. remove docs and images folders

4. in project root use:
```
install.packages("usethis")
usethis::use_tidy_github_actions()
```

5. follow the instructions:
- accept to delete `.travis.yml` and `appveyor.yml`
- copy the new CHECK badge from the console to the `README.md` (instead of travis badge)

6. in`.github/workflows/pkgdown.yaml`:

section `Install dependencies>run`:
- add `remotes::install_github("ModelOriented/DrWhyTemplate")` after the `remotes::install_dev("pkgdown")` line

7. in `.github/workflows/R-CMD-check.yaml`:

section `on>push>branches`:
- add branches/file extensions/folders that should trigger R-CMD-check
- branches: `master`, `'dev-*'` , `'fix*'`, `'!python-*'`, `'!doc'` (not python, not doc)
- paths: `'**.R'`, `'DESCRIPTION'`, `'**.Rmd'` (possible: `'**.py'`, `'**.js'`)
- A LOT of possibilities explained in detail [here](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions)

section `jobs>R-CMD-check>strategy>matrix>config`:
- **remove environments (e.g. Rv3.2, Rv3.3, Rv3.4 if the package depends on R (>= 3.5))**
- add more environments if desired

(**advised**) section `name: Check` (ctrl+f `rcmdcheck`):
- add args to `rcmdcheck` function

`rcmdcheck::rcmdcheck(args = c("--no-manual", "--as-cran", "--run-donttest", "--run-dontrun"), ...)`

8. (**advised**) make `.gitattributes` file in project root and paste:
```
docs/* linguist-documentation
man/* linguist-documentation
misc/* linguist-documentation
pkgdown/* linguist-documentation
```

9. (optional) put `CONTRIBUTING.md` into the `.github` folder

10. (optional) in `README.md` R CHECK badge add `?query=workflow%3AR-CMD-check` to the second link

(link to `Actions>R-CMD-check` not `Actions`)

## After push:

Change the GitHub Pages website location in repository settings to `gh-pages` branch.
