Raine's fork

- Upgrade dependencies so it works on node >= v12
- Use `AWS_ENDPOINT` environment variable or `endpoint` config key to override endpoint, e.g. nyc3.digitaloceanspaces.com

---

# s3 cli

Command line utility frontend to [node-s3-client](https://github.com/andrewrk/node-s3-client).
Inspired by [s3cmd](https://github.com/s3tools/s3cmd) and attempts to be a
drop-in replacement.

## Features

- Compatible with [s3cmd](https://github.com/s3tools/s3cmd)'s config file
- Supports a subset of s3cmd's commands and parameters
  - including `put`, `get`, `del`, `ls`, `sync`, `cp`, `mv`
- When syncing directories, instead of uploading one file at a time, it
  uploads many files in parallel resulting in more bandwidth.
- Uses multipart uploads for large files and uploads each part in parallel.
- Retries on failure.

## Install

`sudo npm install -g s3-cli`

## Configuration

s3-cli is compatible with s3cmd's config file, so if you already have that
configured, you're all set. Otherwise you can put this in `~/.s3cfg`:

```ini
[default]
access_key = foo
secret_key = bar
```

You can also point it to another config file with e.g. `$ s3-cli --config /path/to/s3cmd.conf`.

## Documentation

### put

Uploads a file to S3. Assumes the target filename to be the same as the source filename (if none specified)

Example:

```
s3-cli put /path/to/file s3://bucket/key/on/s3
s3-cli put /path/to/source-file s3://bucket/target-file
```

Options:

- `--acl-public` or `-P` - Store objects with ACL allowing read for anyone.
- `--default-mime-type` - Default MIME-type for stored objects. Application
  default is `binary/octet-stream`.
- `--no-guess-mime-type` - Don't guess MIME-type and use the default type
  instead.
- `--add-header=NAME:VALUE` - Add a given HTTP header to the upload request. Can be
  used multiple times. For instance set 'Expires' or 'Cache-Control' headers
  (or both) using this options if you like.
- `--region=REGION-NAME` - Specify the region (defaults to us-east-1)

### get

Downloads a file from S3.

Example:

```
s3-cli get s3://bucket/key/on/s3 /path/to/file
```

### del

Deletes an object or a directory on S3.

Example:

```
s3-cli del [--recursive] s3://bucket/key/on/s3/
```

### ls

Lists S3 objects.

Example:

```
s3-cli ls [--recursive] s3://mybucketname/this/is/the/key/
```

### sync

#### Sync a local directory to S3

Example:

```
s3-cli sync [--delete-removed] /path/to/folder/ s3://bucket/key/on/s3/
```

Supports the same options as `put`.

#### Sync a directory on S3 to disk

Example:

```
s3-cli sync [--delete-removed] s3://bucket/key/on/s3/ /path/to/folder/
```

### cp

Copy an object which is already on S3.

Example:

```
s3-cli cp s3://sourcebucket/source/key s3://destbucket/dest/key
```

### mv

Move an object which is already on S3.

Example:

```
s3-cli mv s3://sourcebucket/source/key s3://destbucket/dest/key
```
