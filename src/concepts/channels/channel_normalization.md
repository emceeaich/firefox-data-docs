# Telemetry Channel Behavior

In every ping there are two channels:
- App Update Channel
- Normalized Channel 

## Expected Channels
The traditional channels we expect are:
- `release`
- `beta`
- `aurora` (this is `dev-edition`, and [is just a beta repack](https://developer.mozilla.org/en-US/Firefox/Developer_Edition))
- `nightly`
- `esr`

## App Update Channel
This is the channel reported by the application directly. This could really be anything, but is usually one of the
expected release channels listed above.

For BigQuery tables corresponding to Telemetry Ping types, such as `main`, `crash` or `event`, the field here is called `app_update_channel` and is found in `metadata.uri`. For example:
```
SELECT
  metadata.uri.app_update_channel
FROM
  telemetry.main
WHERE
  DATE(submission_timestamp) = '2019-09-01'
LIMIT
  10
```

## Normalized Channel
This field is a normalization of the directly reported channel, and replaces unusual and unexpected values with the string `Other`. There are a couple of exceptions, notably that variations on `nightly-cck-*` become `nightly`.
[See the relevant code here](https://github.com/mozilla/gcp-ingestion/blob/92ba503c4debc887e746d5f2ff5ee60becb8072f/ingestion-beam/src/main/java/com/mozilla/telemetry/transforms/NormalizeAttributes.java#L38).

Normalized channel is available in the Telemetry Ping tables as a top-level field called `normalized_channel`. For example:
```
SELECT
  normalized_channel
FROM
  telemetry.crash
WHERE
  DATE(submission_timestamp) = '2019-09-01'
LIMIT
  10
```
