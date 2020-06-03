# PMM Settings Page

{{ pmm_settings }} is a special page dedicated to configuring a number of PMM
options. It can be accessed through the main menu:

{{ pmm_settings }} page consists of the following sections, which allow to configure
different aspects of the PMM Server:

## Settings

*Settings* section allows you to change [metrics resolution](https://www.percona.com/doc/percona-monitoring-and-management/2.x/faq.html#what-resolution-is-used-for-metrics), [data retention](https://www.percona.com/doc/percona-monitoring-and-management/2.x/faq.html#how-to-control-data-retention-for-pmm),
as well as configure [telemetry](https://www.percona.com/doc/percona-monitoring-and-management/2.x/glossary-terminology.html#telemetry) and automatic checking for [updates](https://www.percona.com/doc/percona-monitoring-and-management/2.x/glossary-terminology.html#PMM-Version):

Don’t forget to click the *Apply changes* button to make changed options work.

## Metrics resolution

{{ pmm }} collects metrics at three different time intervals.

Short time intervals are regarded as high resolution metrics,
while those at longer time intervals are low resolution.

The Metrics Resolution slider lets you choose
from three preset combinations of intervals corresponding
to high, medium, and low resolution
(short, medium, and long collection periods).

The presets are also labeled Low, Medium, and High.

The slider tool-tip shows the collection time corresponding to each
resolution.


* Setting the slider to Low increases the time between collection, resulting in low-resolution metrics.


* Setting the slider to High decreases the time between collection, resulting in high-resolution metrics.

## Telemetry

The *Telemetry* switch enables gathering and sending basic **anonymous** data to
Percona, which helps us to determine where to focus the development
and what is the uptake of the various versions of PMM. Specifically, gathering
this information helps determine if we need to release patches to legacy
versions beyond support, determining when supporting a particular version is no
longer necessary, and even understanding how the frequency of release encourages
or deters adoption.

Currently, only the following information is gathered:


* PMM Version,


* Installation Method (Docker, AMI, OVF),


* the Uptime.

We do not gather anything that would make the system identifiable, but the
following two things are to be mentioned:


1. The Country Code is evaluated from the submitting IP address before it is
discarded.


2. We do create an “instance ID” - a random string generated using UUID v4.
This instance ID is generated to distinguish new instances from existing
ones, for figuring out instance upgrades.

**NOTE**: The first telemetry reporting of a new PMM Server instance is delayed
by 24 hours to allow sufficient time to disable the service for those that do
not wish to share any information.

There is a landing page for this service, available at [check.percona.com](https://check.percona.com),
which clearly explains what this service is, what it’s collecting, and how you
can turn it off.

**NOTE**: The [Grafana internal reporting feature](https://grafana.com/docs/grafana/latest/installation/configuration/#reporting-enabled) is currently **not** managed by PMM. If you want to turn it, you need to go inside the PMM Server container and [change configuration](https://grafana.com/docs/grafana/latest/installation/configuration/#reporting-enabled) after each update.

**NOTE**: Beside using *PMM Settings* page, you can also disable Telemetry with the `-e DISABLE_TELEMETRY=1` option in your docker run statement for the PMM Server.

## Check for updates

When active, {{ pmm }} will automatically check for updates and put a notification
in the Updates dashboard if any are available.

## Security Threat Tool

{{ stt }} performs a range of security-related
checks on a registered instance and reports the findings.

{{ stt }} is disabled by default.

It can be enabled in PMM ‣ PMM Settings ‣ Settings ‣ Advanced Settings ‣ Security Threat Tool.

The checks take 24 hours to complete.

The results can be viewed in PMM ‣ PMM Database Checks.

## SSH Key Details

This section allows you to upload your public SSH key which can be used to
access the PMM Server via SSH (e.g. the [PMM Server deployed as a virtual appliance](/install/virtual-appliance.md#pmm-deploying-server-virtual-appliance-accessing)).

Submit your **public key** in the *SSH Key* field and click the
*Apply SSH Key* button.

## Prometheus Alertmanager integration

Prometheus Alertmanager manages alerts from Prometheus,
deduplicating, grouping, and routing them
to the appropriate receiver or display component.

This section allows you to configure [integration of Prometheus with an external Alertmanager](https://www.percona.com/doc/percona-monitoring-and-management/2.x/faq.html#how-to-integrate-alertmanager-with-pmm).


* The **Alertmanager URL** field should contain the URL of the Alertmanager
which would serve your PMM alerts.


* The **Prometheus Alerting rules** field is used to specify alerting rules in the YAML
configuration format.

Fill both fields and click the *Apply Alertmanager settings* button to proceed.

## Diagnostics

{{ pmm }} can generate a set of diagnostics data which can be examined
and/or shared with Percona Support in case of some issue to solve it faster.
You can [get collected logs from PMM Server](https://www.percona.com/doc/percona-monitoring-and-management/2.x/faq.html#id13) by clicking
the **Download PMM Server Logs** button.