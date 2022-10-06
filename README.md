<p align="center">
    <a href="https://github.com/open-turo/actions-s3-artifact/releases/" alt="Release">
        <img src="https://img.shields.io/github/release/open-turo/actions-s3-artifact" /></a>
    <a href="https://github.com/open-turo/actions-s3-artifact/releases/" alt="Release date">
        <img src="https://img.shields.io/github/release-date/open-turo/actions-s3-artifact" /></a>
    <a href="https://github.com/open-turo/actions-s3-artifact/releases/" alt="Release status">
        <img src="https://github.com/open-turo/actions-s3-artifact/actions/workflows/release.yaml/badge.svg " /></a>
    <a href="https://github.com/open-turo/actions-s3-artifact/blob/main/LICENSE" alt="License">
        <img src="https://img.shields.io/github/license/open-turo/actions-s3-artifact.svg" /></a>
    <a href="https://GitHub.com/open-turo/actions-s3-artifact/commit/" alt="Last commit">
        <img src="https://img.shields.io/github/last-commit/open-turo/actions-s3-artifact" /></a>
    <a href="https://conventionalcommits.org" alt="Conventional commits">
        <img src="https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg" /></a>
</p>

# `open-turo/actions-s3-artifact`

GitHub Action: manage S3 artifacts in workflows

## Actions

### action: [`upload`](./upload)

Upload will tarball and upload artifact(s) to S3 and optionally manage
authentification if AWS credentials are specified.

See usage [here](./upload/README.md#usage).

Documentation is found [here](./upload/README.md).

### action: [`download`](./download)

Download will download artifacts from s3 path and optionally extract files from
tar if s3 path ends with `.tgz` or `.tar.gz`.

See usage [here](./download/README.md#usage).

Documentation is found [here](./download/README.md).

## Get Help

Each Action has a detailed README for how to use it as referenced above. Please review Issues, post new Issues against this repository as needed.

## Contributions

Please see [here](https://github.com/open-turo/contributions) for guidelines on how to contribute to this project.

## Use cases

- actions-s3-artifact/upload
  - Upload a build artifact to S3 for future use externally
    - Uploading an ML model to use in a container
    - Uploading a Javascript bundle for serving
  - Upload an artifact for future use in another job
    - Uploading a preprocessed data set for use in a training job
    - Uploading a javascript build for use in parallel jobs
- actions-s3-artifact/download
  - Needs to be able to fail without erroring the pipeline for a cache-like behavior
  - Needs to be able to output its success status for future pipeline steps
  - Download an external artifact from S3 for use in a job
    - Downloading data set to work with in CI/CD
    - Downloading ML model to build into deployment CD
  - Download an artifact from a previous job for use in a job
    - Downloading a preprocessed data set for use in a training job
    - Downloading a javascript build for use in parallel jobs
- actions-s3-artifact
  - TBD: Might not be worth it
  - Cache and retrieve an artifact based on a possibly changing key or hash of a file
  - Check cache
  - If it's present, skip computation
  - If it's not present, compute
  - Upload to cache, if it was not retrieved
