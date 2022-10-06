## Description

# Github Action S3 Artifact upload

This action uploads one or multiple files to an s3 uri location.

## Usage

The following section contains examples on the usage of this action.

### Authorizing

All of these actions require some form of AWS authorization.

#### Authorizing for multiple steps

If you are authorizing for multiple artifact upload/download steps, it is
advised to authorize once at the start of your workflow job.

Here is an example authorization step:

```yaml
- uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: ${{ secrets.AWS_REGION }}
```

Once authorized all the `open-turo/actions-s3-artifacts/*` actions can be used.

#### Authorizing for a single step

All of the actions in this group take arguments for authorizing AWS S3 access
when they are used singularly. The inputs available are:

- `aws-access-key-id` - AWS access key ID
- `aws-secret-access-key` - AWS secret access key
- `aws-region` - AWS region

Both `aws-access-key-id` and `aws-secret-access-key` are required to trigger the
authorization process.

### Upload

#### Minimal upload

Minimally configured action if already authorized S3/AWS credentials. This will
tarball and upload the file `bar/foo/artifact.xyz` to
`s3://bucket-name/path/to/file` and will use the default key, namely
${{github.sha}}-${{github.run_number}}-${{github.run_attempt}} as name.

```yaml
- uses: open-turo/actions-s3-artifact/upload@v1
  with:
    path: bar/foo/artifact.xyz
    s3uri: s3://bucket-name/path/to/file
```

#### Upload with authorization

The following example shows how to upload a file with authorization. This will
create AWS credentials to use with S3 and will
tarball and upload the file `bar/foo/artifact.xyz` to
`s3://bucket-name/path/to/file` and will use the default key, namely
${{github.sha}}-${{github.run_number}}-${{github.run_attempt}} as name.

```yaml
- uses: open-turo/actions-s3-artifact/upload@v1
  with:
    path: bar/foo/artifact.xyz
    s3uri: s3://bucket-name/path/to/file
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: ${{ secrets.AWS_REGION }}
```

#### Upload with specific key name

The following example shows how to upload a file with authorization. This will
create AWS credentials to use with S3 and will tarball and upload the file
`bar/foo/artifact.xyz` to `s3://bucket-name/path/to/file` and will use the given
key `YYYY-MM-DD` as name.

```yaml
- uses: open-turo/actions-s3-artifact/upload@v1
  with:
    path: bar/foo/artifact.xyz
    s3uri: s3://bucket-name/path/to/file
    key: YYYY-MM-DD
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: ${{ secrets.AWS_REGION }}
```

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
