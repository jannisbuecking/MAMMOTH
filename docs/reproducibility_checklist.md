# Reproducibility Checklist

This repository contains the deployed MAMMOTH Shiny app, app-packaged data objects, and environment metadata.

## App-safety note

The Posit/Shiny app depends on the current root-level layout:

- `app.R`
- `manifest.json`
- `requirements.txt`
- `secondary_data.RData`
- `data_arrow/`
- `www/`

Do not move these files unless `app.R` and Posit deployment settings are updated at the same time.

## Environment records

- `manifest.json`: Posit deployment manifest.
- `renv.lock`: package lockfile generated from the local R app environment.
- `session_info/sessionInfo.txt`: plain-text R `sessionInfo()` output.
- `session_info/detected_package_versions.tsv`: versions of packages detected from `library()` and `require()` calls in `app.R`.

The lockfile was generated with R 4.5.1 and Bioconductor 3.21. During lockfile generation, repository metadata could not be queried from CRAN/Bioconductor in the sandboxed environment, so `renv::snapshot(..., force = TRUE)` was used to record the installed local package state.

## Run locally

From the repository root:

```r
shiny::runApp(".")
```

## Restore R environment

From the repository root:

```r
install.packages("renv")
renv::restore()
```

The Posit deployment manifest remains the authoritative deployment record for the live app.

## Update environment records

After testing the app in the final deployment environment:

```r
renv::snapshot(prompt = FALSE, force = TRUE)
sink("session_info/sessionInfo.txt")
sessionInfo()
sink()
```

Commit the updated `renv.lock`, `manifest.json` if redeployed, and `session_info/sessionInfo.txt`.
