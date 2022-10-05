     _       ____   _____   ___    ___    _   _           ____     ___     ____   ____
    / \     / ___| |_   _| |_ _|  / _ \  | \ | |         |  _ \   / _ \   / ___| / ___|

/ \_ \ | | | | | | | | | | | \| | **\_** | | | | | | | | | | \_** \
 / \_** \ | |**_ | | | | | |_| | | |\ | |\_\_\_**| | |_| | | |_| | | |**\_ \_**) |
/\_/ \_\ \_**_| |_| |\_**| \_**/ |\_| \_| |\_\_**/ \_**/ \_\_**| |\_\_\_\_/

## Description

Download a set of artifacts from S3

## Inputs

| parameter             | description                                                   | required | default   |
| --------------------- | ------------------------------------------------------------- | -------- | --------- |
| path                  | Path to download artifacts to                                 | `false`  | .         |
| strip                 | Strip leading path components from downloaded artifacts       | `false`  |           |
| s3uri                 | S3 uri to artifact to download                                | `true`   |           |
| not-found             | What to do if the artifact is not found (error, warn, ignore) | `false`  | error     |
| aws-access-key-id     | AWS access key ID of the S3 location                          | `false`  |           |
| aws-secret-access-key | AWS secret access key ID of the S3 location                   | `false`  |           |
| aws-region            | AWS region of the S3 location                                 | `false`  | us-east-1 |

## Outputs

| parameter | description                                  |
| --------- | -------------------------------------------- |
| s3uri     | S3 URL for uploaded artifact                 |
| success   | Whether the artifact download was successful |

## Runs

This action is a `composite` action.
