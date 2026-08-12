# OneDrive client for Linux
*Instructions on how to use onedrive client on Linux*

Microsoft OneDrive provided by the University of Bern does not allow access via `rclone`. This makes it less convenient to transfer files programmatically on an HPC.
A workaround is to use the [onedrive client](https://github.com/abraunegg/onedrive) for linux.

Below are instructions on how to set it up and use it on UBELIX; however, the same applies to any Linux machine.

### Singularity image
The image is located at `dbmr_luisierlab/resources/img/onedrive/onedrive_2.5.11.sif`.

It was built simply by pulling the docker image from driveone/onedrive:2.5.11.

###  Setup
Before using it for the first time, you should provide access to your OneDrive. You need to do this only once. Execute `onedrive` with no arguments:
```
singularity exec onedrive_2.5.11.sif onedrive
```
... and follow the instructions. Basically, you will be given a link, which you should copy into a web browser on your local machine. Once you log in, you will be forwarded to a blank page. Copy the entire link from the browser and paste it into the terminal.

![copy_redirect_uri_to_application](https://github.com/abraunegg/onedrive/raw/master/docs/images/authorise_client_before_copy_with_arrow.png)

> [!NOTE]
> Be quick to copy the link because the website will redirect within a few seconds!

### Usage
> [!IMPORTANT]
> By default `onedrive` uses 8 cpu's and can take some time when dealing with large data. Execute it on a compute node in an interactive session!

By default, `onedrive` will sync your entire OneDrive to `~/OneDrive`:
```
singularity exec onedrive_2.5.11.sif onedrive --sync
```

> [!NOTE]
> * To be on the safe side, you can use the `--download-only` option.
> * If you were given shared data from another OneDrive account, you should first add a shortcut to your OneDrive by clicking on the three dots `...` next to the folder and choosing `Add shortcut to My files`→`Shortcuts`. You should also include the `--sync-shared-files` option.

### Customisation and tips
It's a good practice to enable logging by including the options `--enable-logging` and `--log-dir`.

Use the `--dry-run` option to see what will happen. You can also redirect the output to a file.

If you want to sync only specific folders from OneDrive, the client automatically looks for a file named `sync_list` inside its configuration directory:
`~/.config/onedrive/sync_list`

Its content should look like this:
```
/Documents/WorkProject
/ProjectX/row_data
```

In most cases, we want to transfer a specific folder from OneDrive to a specific location on the HPC.
Use the options
```
--single-directory <OneDrive path, no leading slash>
--syncdir /local/folder
```
> [!NOTE]
> The client might prompt you to resync; choose Y.

The same principles should be applicable when uploading data to your OneDrive. See the [online documentation](https://github.com/abraunegg/onedrive/blob/master/docs/usage.md) for more details.

> [!TIP]
> To change the default options, you can copy [this file](https://raw.githubusercontent.com/abraunegg/onedrive/refs/heads/master/config) to `~.config/onedrive`, and comment out and change any option (eg., `enable_logging`, `log_dir`, `sync_business_shared_items`, `threads`, etc).
