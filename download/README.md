# Github Action S3 Artifact Download

<!-- prettier-ignore-start   -->
<!-- action-docs-description -->

## Description

GitHub Action that builds Node based repository

<!-- prettier-ignore-end -->

## Usage

The following section contains examples on the usage of this action.

### Authorizing

All of these actions require some form of AWS authorization, except download
from public S3 buckets, which is not recommended.

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

### Download

#### Minimal download

Minimally configured action if already authorized S3/AWS credentials. This
will download "file" into the current working directory, preserving the name
"file".

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:
    s3uri: s3://bucket-name/path/to/file
```

#### Download with authorization

The following example shows how to download a file with authorization. This will
create AWS credentials to use with S3 and place the file at the current
directory.

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:
    s3uri: s3://bucket-name/path/to/file
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: ${{ secrets.AWS_REGION }}
```

#### Download tarball to current directory

The following example shows how to download a tarball and extract it to the
current working directory. Note the `.tgz` extension on the S3 path. This is
required for automated un-tarballing. `.tar.gz` is also supported.

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:
    s3uri: s3://bucket-name/path/to/archive.tgz
```

#### Download tarball to specific directory

The following example shows how to download a tarball and extract it to a
specific path. Note the `.tgz` extension on the S3 path. This is required for
automated un-tarballing. `.tar.gz` is also supported.

This will preserve the paths from the tarball into the `artifacts/` directory
created within the current working directory.

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:
    path: artifacts
    s3uri: s3://bucket-name/path/to/archive.tgz
```

#### Download tarball and strip loading paths

The following example shows how to download a tarball and extract it to a
specific path. Note the `.tgz` extension on the S3 path. This is required for
automated un-tarballing. `.tar.gz` is also supported.

This will strip 1 level of path from the tarball into the `artifacts/` directory
created within the current working directory. This is identical to the
`tar --strip-components=1` behavior.

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:`
    s3uri: s3://bucket-name/path/to/archive.tgz
    path: artifacts
    strip: 1
```

#### Attempt to download but ignore failure

This example shows how to make a passthrough failure download attempt.

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:
    s3uri: s3://bucket-name/path/to/does-not-exist
    path: artifacts
    not-found: ignore
```

#### Download from public S3 bucket

The following example shows how to download from a public S3 bucket without authentication:

```yaml
- uses: open-turo/actions-s3-artifact/download@v1
  with:
    s3uri: s3://public-bucket-name/path/to/file
    public-bucket: true
```

#### Download artifacts pushed with the upload action

Complete workflow example showing upload and download of artifacts:

```yaml
jobs:
  upload:
    runs-on: ubuntu-latest
    env:
      S3_BUCKET: my-artifacts-bucket
    steps:
      # Upload individual files
      - name: Upload individual files
        id: individual
        uses: open-turo/actions-s3-artifact/upload@v1
        with:
          s3uri: s3://${{ env.S3_BUCKET }}/test-upload
          path: |
            upload/action.yaml
            download/action.yaml

      # Upload directory contents with glob
      - name: Upload directory contents with glob
        id: glob
        uses: open-turo/actions-s3-artifact/upload@v1
        with:
          s3uri: s3://${{ env.S3_BUCKET }}/test-upload-glob
          path: .github/*

      # Upload with specified key
      - name: Upload with specified key
        id: key
        uses: open-turo/actions-s3-artifact/upload@v1
        with:
          s3uri: s3://${{ env.S3_BUCKET }}/test-upload-key
          key: ${{ hashFiles('upload/action.yaml') }}
          path: upload/*

  download:
    runs-on: ubuntu-latest
    needs: upload
    env:
      S3_BUCKET: my-artifacts-bucket
    steps:
      # Download tar archive
      - name: Download tar
        id: tar
        uses: open-turo/actions-s3-artifact/download@v1
        with:
          s3uri: s3://${{ env.S3_BUCKET }}/test-upload/artifact.tgz
          path: artifact

      # Download file to default path
      - name: Download file to default path
        id: file-default-path
        uses: open-turo/actions-s3-artifact/download@v1
        with:
          s3uri: s3://bucket-name/path/to/file.zip

      # Download file to specified path
      - name: Download file to specified path
        id: file-specified-path
        uses: open-turo/actions-s3-artifact/download@v1
        with:
          s3uri: s3://bucket-name/path/to/file.zip
          path: artifact/downloads

      # Download tree and strip paths
      - name: Download tree and strip paths
        id: download-tree-strip
        uses: open-turo/actions-s3-artifact/download@v1
        with:
          s3uri: s3://${{ env.S3_BUCKET }}/test-upload-glob/artifact.tgz
          path: artifact-tree-strip
          strip: 1
```

<!-- prettier-ignore-start -->
<!-- action-docs-inputs source="action.yaml"  -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| checkout-repo | Perform checkout as first step of action | `false` | true |
| build-script | Custom script to run, should be defined in package.json. | `false` | build |
| github-token | GitHub token that can checkout the repository. e.g. 'secrets.GITHUB_TOKEN' | `true` | ${{ github.token }} |
| npm-auth-token | The Node Package Manager (npm) authentication token. This token is used to authenticate against a private NPM registry configured via a .npmrc file. | `false` |  |
| npm-token | The Node Package Manager (npm) authentication token. This token is used to authenticate against the NPM registry. | `false` |  |
<!-- action-docs-outputs source="action.yaml"  -->
<!-- action-docs-runs source="action.yaml"  -->
## Runs

This action is a `composite` action.
<!-- action-docs-usage source="action.yaml" -->
## Usage

```yaml
- uses: @
  with:
    path:
    # Path to download artifacts to
    #
    # Required: false
    # Default: .

    strip:
    # Strip leading path components from downloaded artifacts
    #
    # Required: false
    # Default: ""

    s3uri:
    # S3 uri to artifact to download
    #
    # Required: true
    # Default: ""

    not-found:
    # What to do if the artifact is not found (error, warn, ignore)
    #
    # Required: false
    # Default: error

    role-to-assume:
    # ARN of the role to assume. If no credentials are provided, the action will not try to authenticate with AWS and use any existing environment credentials.
    #
    # Required: false
    # Default: ""

    aws-access-key-id:
    # AWS access key ID of the S3 location. If no credentials are provided, the action will not try to authenticate with AWS and use any existing environment credentials.
    #
    # Required: false
    # Default: ""

    aws-secret-access-key:
    # AWS secret access key ID of the S3 location. If no credentials are provided, the action will not try to authenticate with AWS and use any existing environment credentials.
    #
    # Required: false
    # Default: ""

    aws-region:
    # AWS region of the S3 location
    #
    # Required: false
    # Default: us-east-1

    public-bucket:
    # Enable access to public S3 buckets without authentication (uses --no-sign-request)
    #
    # Required: false
    # Default: false
```
<!-- action-docs-usage source="action.yaml" -->
<!-- prettier-ignore-end -->
