# {{ dbd_prometheus }} Dashboard

The {{ dbd_prometheus }} dashboard informs how {{ prometheus }} functions.

## {{ prometheus }} overview

This section shows the most essential parameters of the system where
{{ prometheus }} is running, such as CPU and memory usage, scrapes performed and the
samples ingested in the head block.

## Resources

This section provides details about the consumption of CPU and memory by the
{{ prometheus }} process. This section contains the following metrics:

## Storage (TSDB)

This section includes a collection of metrics related to the usage of
storage. It includes the following metrics:

## Scraping

This section contains metrics that help monitor the scraping process. This
section contains the following metrics:

## Queries

This section contains metrics that monitor {{ prometheus }} queries. This section
contains the following metrics:

## Network

Metrics in this section help detect network problems.

## Time Series Information

This section shows the top 10 metrics by time series count and the top 10 hosts
by time series count.

## System Level Metrics

Metrics in this section give an overview of the essential system characteristics
of {{ pmm_server }}. This information is also available from the {{ dbd_system_overview }}
dashboard.

## {{ pmm }} Server Logs

This section contains a link to download the logs collected from your
{{ pmm_server }} and further analyze possible problems. The exported logs are
requested when you submit a bug report.
