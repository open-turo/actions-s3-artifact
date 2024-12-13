# Github Action S3 Artifact upload

<!-- prettier-ignore-start   -->
<!-- action-docs-description -->

## Description

GitHub Action that builds Node based repository

<!-- prettier-ignore-end -->

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
    # Path(s) to the artifacts to upload
    #
    # Required: true
    # Default: ""

    s3uri:
    # S3 url for bucket and path prefix of the artifact
    #
    # Required: true
    # Default: ""

    key:
    # Artifact key name (a unique hash or timestamp or other identifier)
    #
    # Required: false
    # Default: ${{ github.sha }}-${{ github.run_number }}-${{ github.run_attempt }}

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

    compress:
    # Whether to build a tarball of the artifacts before uploading
    #
    # Required: false
    # Default: true
```
<!-- action-docs-usage source="action.yaml" -->
<!-- prettier-ignore-end -->
