# AWS CLI

To setup a default user

```
aws configure
```

To setup a profile

```
aws configure --profile <profile name>
```

*AWS Access Key ID* and *AWS Secret Access Key* are stored in
`~/.aws/credentials`. These are associated with an IAM user or role.

All other configuration is stored in `~/.aws/config`.

Get default user credentials

```
aws sts get-caller-identity
aws sts get-caller-identity --profile <profile name>
```

List profiles
```
aws configure list-profiles
```

## Files

- *~/.aws/config* non-sensitive information
```
[default]
region = eu-north-1
output = yaml
```

- *~/.aws/credentials* sensitive information
```
[default]
aws_access_key_id = <access key>
aws_secret_access_key = <secret access key>
```

## Profiles

Use

- `--profile` flag, or
- `AWS_PROFILE` environment variable.

Eg,

```
aws s3 ls --profile profile_name
```

Add 

```
[profile_name]
```

to *~/.aws/config*, and

```
[profile profile_name]
```

to *~/.aws/credentials*.

**Note**: `[profile ...]` in *~/.aws/credentials* file.
