# actions-s3-artifact

GitHub Action: manage S3 artifacts in workflows

Put docs here.

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
