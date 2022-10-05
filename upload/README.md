     _       ____   _____   ___    ___    _   _           ____     ___     ____   ____
    / \     / ___| |_   _| |_ _|  / _ \  | \ | |         |  _ \   / _ \   / ___| / ___|

/ \_ \ | | | | | | | | | | | \| | **\_** | | | | | | | | | | \_** \
 / \_** \ | |**_ | | | | | |_| | | |\ | |\_\_\_**| | |_| | | |_| | | |**\_ \_**) |
/\_/ \_\ \_**_| |_| |\_**| \_**/ |\_| \_| |\_\_**/ \_**/ \_\_**| |\_\_\_\_/

## Description

Upload a set of artifacts to S3

## Inputs

| parameter             | description                                                        | required | default                                                              |
| --------------------- | ------------------------------------------------------------------ | -------- | -------------------------------------------------------------------- |
| path                  | Path(s) to the artifacts to upload                                 | `true`   |                                                                      |
| s3uri                 | S3 url for bucket and path prefix of the artifact                  | `true`   |                                                                      |
| key                   | Artifact key name (a unique hash or timestamp or other identifier) | `false`  | ${{ github.sha }}-${{ github.run_number }}-${{ github.run_attempt }} |
| aws-access-key-id     | AWS access key ID of the S3 location                               | `false`  |                                                                      |
| aws-secret-access-key | AWS secret access key ID of the S3 location                        | `false`  |                                                                      |
| aws-region            | AWS region of the S3 location                                      | `false`  | us-east-1                                                            |

## Outputs

| parameter | description                  |
| --------- | ---------------------------- |
| s3uri     | S3 URL for uploaded artifact |

## Runs

This action is a `composite` action.
