# pull

Pull data from `provider` API endpoints.

Supports downloading into the Docker container (e.g. in a mounted directory or volume), and also storing in Amazon S3.

## Running

Run the container to pull data from one or more sources.

First, copy and edit the sample configuration file:

```bash
$ cp .config.sample .config
```

Next, ensure the image is up to date locally:

```bash
$ docker-compose build --no-cache pull
```

Then run the data puller:

```bash
$ docker-compose run --rm pull [--status_changes] [--trips] [OPTIONS]
```

### [OPTIONS]

Note: you must provide a range of time to query using some combination of `start_time`, `end_time`, and `duration`. Providing both `start_time` and `end_time` takes precedence over either of them with `duration`.

Options include:

#### `--aws_region REGION`

The AWS region to use for S3 uploads. Only applies when given with the `--s3_bucket` argument. Overrides the `AWS_DEFAULT_REGION` environment variable. If `AWS_DEFAULT_REGION` is not set, this parameter must be given.

#### `--bbox BBOX`

The bounding-box with which to restrict the results of this request.

The order is (separated by commas):
  - southwest longitude,
  - southwest latitude,
  - northeast longitude,
  - northeast latitude

For example:

```
--bbox -122.4183,37.7758,-122.4120,37.7858
```

#### `--config CONFIG`

Path to a provider configuration file to use. The default is `.config`.

#### `--device_id DEVICE_ID`

The device_id to obtain results for. Only applies to `--trips`.

#### `--duration DURATION`

Number of seconds; with `--start_time` or `--end_time` defines a time query range.

#### `--end_time END_TIME`

The end of the time query range for this request.
Should be either int Unix seconds or ISO-8061 datetime format.

#### `--no_paging`

Flag indicating paging through the response should *not* occur. Return only the first page of data.

#### `--output OUTPUT`

Base path to write data files into.

#### `--providers PROVIDER [PROVIDER ...]`

One or more providers to query, identified either by `provider_name` or `provider_id`.

The default is to query all configured providers.

#### `--ref REF`

Git branch name, commit hash, or tag at which to reference MDS. The default is `master`.

#### `--s3_bucket BUCKET`

Name of an AWS S3 bucket to store data files in. When used with `--output`, a common key prefix is given to data files.

AWS credentials must be configured; set the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables or use standard AWS credential configuration in e.g. `~/.aws/credentials`.

#### `--start_time START_TIME`

The beginning of the time query range for this
request. Should be either int Unix seconds or ISO-8061 datetime format.

#### `--status_changes`

Flag indicating Status Changes should be requested.

#### `--trips`

Flag indicating Trips should be requested.

#### `--vehicle_id VEHICLE_ID`

The `vehicle_id` to obtain results for. Only applies to `--trips`.

### Storing in Amazon S3

To store data files in an S3 bucket, configure your environment with the following variables:

```
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=
```

Then pass the bucket name as one of the options:

```console
--s3_bucket my-bucket
```