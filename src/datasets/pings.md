# Raw Ping Data

<!-- toc -->

# Introduction

{{#include ./ping_intro.md}}

# Data Reference

You can find the reference documentation for all ping types
[here](https://firefox-source-docs.mozilla.org/toolkit/components/telemetry/telemetry/concepts/pings.html).

# Ping Metadata

The telemetry data pipeline appends metadata to arriving pings containing
information about the ingestion environment, including timestamps;
Geo-IP data about the client;
and fields extracted from the ping or client headers that are useful for downstream processing.

The [Dataset API](https://mozilla.github.io/python_moztelemetry/api.html#dataset)
represents this metadata by appending a `meta` key to the ping body.
These fields may also be available as members of a `metadata` struct column
in direct-to-parquet datasets.

Since the metadata are not present in the ping as it is sent by the client,
these fields are documented here, instead of in the source tree docs.

As of September 28, 2018, members of the `meta` key on main pings include:


Key | Description
--- | -----------
`appBuildId` |
`appName` | e.g. "Firefox"
`appUpdateChannel` | Raw incoming [update channel](../concepts/channels/channel_normalization.md). E.g. `nightly-cck-example`
`appVendor` | e.g. "Mozilla"
`appVersion` |
`clientId` |
`creationTimestamp` | Client `creationDate` field, transformed to nanoseconds since epoch
`Date` | Client HTTP header reflecting the client time when the ping is sent, like `Fri, 28 Sep 2018 14:01:57 GMT`
`DNT` | Client "do not track" HTTP header. Not present in all pings
`docType` | e.g. "main"
`documentId` | A UUID identifying the ping, generated by the client
`geoCity` | from Geo-IP lookup of client IP; `??` if unknown
`geoCountry` | from Geo-IP lookup of client IP; `??` if unknown
`geoSubdivision1` | from Geo-IP lookup of client IP; not present in all pings
`geoSubdivision2` | from Geo-IP lookup of client IP; not present in all pings
`Host` | Public hostname of the ingestion endpoint
`Hostname` | Private hostname of the ingestion endpoint
`normalizedChannel` | Normalized [update channel](../concepts/channels/channel_normalization.md). E.g. "release"
`normalizedOSVersion` |
`os` |
`reason` | Documented [in the source tree](https://firefox-source-docs.mozilla.org/toolkit/components/telemetry/telemetry/data/main-ping.html)
`sampleId` | `crc32(clientId) % 100`
`sourceName` | e.g. "telemetry"
`sourceVersion` | Client `version` field (reflecting the version of the ping format)
`submissionDate` | Server date (GMT) when ping was received, like `20180928`; derived from `Timestamp`
`telemetryEnabled` | Extracted value of `environment.settings.telemetryEnabled`, describing whether opt-in telemetry is enabled
`Timestamp` | Server timestamp when the ping is received, expressed as nanoseconds since epoch
`Type` | e.g. "telemetry"
`X-PingSender-Version` | Present when a ping is sent with [`PingSender`](https://firefox-source-docs.mozilla.org/toolkit/components/telemetry/telemetry/internals/pingsender.html)
