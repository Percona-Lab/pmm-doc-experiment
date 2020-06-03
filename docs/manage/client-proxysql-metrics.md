# Adding a ProxySQL host

## Adding ProxySQL metrics service

Use the {{ opt_proxysql_metrics }} alias
to enable {{ proxysql }} performance metrics monitoring.

### USAGE

```
pmm-admin add proxysql --username=admin --password=admin
```

where username and password are credentials for the monitored MongoDB access,
which will be used locally on the database host. Additionally, two positional
arguments can be appended to the command line flags: a service name to be used
by PMM, and a service address. If not specified, they are substituted
automatically as `<node>-proxysql` and `127.0.0.1:3306`.

The output of this command may look as follows:

> ```
> # pmm-admin add proxysql --username=admin --password=admin
> ProxySQL Service added.
> Service ID  : /service_id/f69df379-6584-4db5-a896-f35ae8c97573
> Service name: ubuntu-proxysql
> ```

Beside positional arguments shown above you can specify service name and
service address with the following flags: `--service-name`, `--host` (the
hostname or IP address of the service), and `--port` (the port number of the
service). If both flag and positional argument are present, flag gains higher
priority. Here is the previous example modified to use these flags:

```
pmm-admin add proxysql --username=pmm --password=pmm --service-name=my-new-proxysql --host=127.0.0.1 --port=3306
```