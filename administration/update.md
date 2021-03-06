# Update

Update ATSD to the latest version by running the `update.sh` script.
The script automates the process of downloading the latest distribution files,
copying files to target directories, and gracefully restarting ATSD services.

:::tip Note
Access to `https://axibase.com` from the target server is required to download installation files. If the server is not connected to the Internet, switch to the [manual update](update-manual.md) option.
:::

## Migration

ATSD for HBase 1.2.5, released in August 2017.

If the **Settings > System Information** page on your current instance displays HBase version `0.94.x`, execute the migration procedure depending on the installation mode:

* [Cluster](migration/cluster.md)
* [Container](migration/container.md)
* [All Other](migration/README.md)

## Check Revision

* Open the **Settings > System Information** page
* Take note of the current Revision Number

## Update Command

```sh
/opt/atsd/bin/update.sh
```

### Update Command in Docker Container

Assuming `atsd` is the name of the container:

```bash
docker exec -it atsd /bin/bash
/opt/atsd/bin/update.sh
```

## Validation

Once the update is completed, the script prints out the following message:

```txt
Update process finished successfully.
```

The update process can take a few minutes to complete.

To verify that ATSD is working correctly, open the ATSD web interface:

```elm
https://atsd_hostname:8443/
```

* Open the **Settings > System Information** page
* Verify that the Revision Number has changed compared to the pre-installation stage

![](./images/revision.png)

## Restart

In the case the web interface is not accessible for more than 5 minutes after the update, initiate a [restart](restarting.md).
