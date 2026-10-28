# betterS3

English | [中文](./README.zh-cn.md)

betterS3 is yet another unofficial sync plugin for Obsidian, forked from [Remotely Save](https://github.com/remotely-save/remotely-save) by fyears. This fork keeps only the free services (S3-compatible storage, WebDAV, Dropbox, OneDrive, Webdis), removes the whole PRO / paid / account system, and makes all former PRO features — including **Smart Conflict** — free for everyone.

## Disclaimer

- **This is NOT the [official sync service](https://obsidian.md/sync) provided by Obsidian.**
- **This plugin is NOT affiliated with any of the supported cloud services.** It just uses their public APIs.

## !!!Caution!!!

**ALWAYS, ALWAYS, backup your vault before using this plugin.**

## Features

- Sync your vault to:
  - Amazon S3 or any S3-compatible storage (Cloudflare R2 / Backblaze B2 / Storj / Tencent Cloud COS / UpYun / MinIO / ...)
  - WebDAV (NextCloud / InfiniCloud / Synology webdav server / ...)
  - Dropbox
  - OneDrive for personal (App Folder)
  - Webdis
  - [Here](./docs/services_connectable_or_not.md) shows more connectable (or not-connectable) services in details.
- **Obsidian Mobile supported.** Vaults can be synced across mobile and desktop devices with the cloud service as the "broker".
- **[End-to-end encryption](./docs/encryption/README.md) supported.** Files are encrypted using the openssl / rclone crypt format before being sent to the cloud **if** the user specifies a password.
- **Scheduled auto sync supported.** You can also manually trigger the sync using the sidebar ribbon, or using the command from the command palette (or even bind a hot key combination to the command).
- **[Minimal Intrusive](./docs/minimal_intrusive_design.md).**
- **Skip large files** and **skip paths** by custom regex conditions.
- **[Sync Algorithm v3](./docs/sync_algorithm/v3/intro.md)** with true deletion detection, deletion / modification protection, backup mode, and no remote meta data.
- **Conflict handling**, all free:
  - keep the newer version (default),
  - keep the larger version,
  - **[Smart Conflict](#smart-conflict-free-now)**: merge small markdown files with the diff3 algorithm, or keep both files by renaming them.
- **Sync Plan Export** for debugging, see [the doc](./docs/how_to_debug/export_sync_plans.md).
- Source Available under the [Apache License 2.0](./LICENSE).

## Limitations

- **Cloud services cost you money.** Always be aware of the costs and pricing. Specifically, all the operations, including but not limited to downloading, uploading, listing all files, calling any api, storage sizes, may or may not cost you money.
- **Some limitations from the browser environment.** More technical details are [in the doc](./docs/browser_env.md).
- **You should protect your `data.json` file.** The file contains sensitive information.
  - It's strongly advised **NOT** to share your `data.json` file to anyone.
  - It's usually **NOT** a good idea to check the file into version control. By default, the plugin tries to create a `.gitignore` file inside the plugin directory if it doesn't exist, for ignoring `data.json` in the `git` version control. If you know exactly what it means and want to remove the setting, please modify the `.gitignore` file or set it to be empty.
- **Obsidian API on Mobile has performance issues syncing large files (>= 50 MB).**
  - Setting the "Skip Large Files" option can help resolve issues related to syncing large files.

## Questions, Suggestions, Or Bugs

Issues and discussions are welcome in the GitHub repo:

- Questions / suggestions: [GitHub repo Discussions](https://github.com/UN-Self/betterS3/discussions)
- Bugs: [GitHub repo Issues](https://github.com/UN-Self/betterS3/issues) (NOT Discussion)

## Download and Install

- Option #1: Search "betterS3" in the Obsidian community plugin list, then install and enable it.
- Option #2: Use [Obsidian42 - BRAT](https://github.com/TfTHacker/obsidian42-brat) and input `UN-Self/betterS3` in the BRAT configuration.
- Option #3: Manually download the assets (`main.js`, `manifest.json`, `styles.css`) from the latest [release](https://github.com/UN-Self/betterS3/releases), and place them into `<your vault>/.obsidian/plugins/<plugin id>/`.
- Option #4: Build it yourself. See the scripts in `package.json` (Node.js and npm are required).

## Usage

### S3

- Tutorials / Examples:
  - [Cloudflare R2](./docs/remote_services/s3_cloudflare_r2/README.md)
  - [Backblaze B2](./docs/remote_services/s3_backblaze_b2/README.md)
  - [Storj](./docs/remote_services/s3_storj_io/README.md)
  - [Tencent Cloud COS](./docs/remote_services/s3_tencent_cloud_cos/README.zh-cn.md) | [Tencent Cloud COS](./docs/remote_services/s3_tencent_cloud_cos/README.md)
  - [MinIO](./docs/remote_services/s3_minio/README.md)
  - [UpYun](./docs/remote_services/s3_upyun/README.zh-cn.md)
- Prepare your S3 (-compatible) service information: [endpoint, region](https://docs.aws.amazon.com/general/latest/gr/s3.html), [access key id, secret access key](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/getting-your-credentials.html), bucket name.
- If you are using AWS S3, create [policy and user](./docs/remote_services/s3_general/s3_user_policy.md).
- Very old versions of Obsidian need [configuring CORS](./docs/remote_services/s3_general/s3_cors_configure.md).
- Download and enable this plugin.
- Enter your information in the settings of this plugin.
- If you do not set the prefix in the settings, the bucket should be empty and solely for syncing one vault. You can set the prefix in the settings so that the same bucket can store multiple vaults.
- If you want to enable end-to-end encryption, also set a password in settings. If you do not specify a password, the files and folders are synced in plain, original content to the cloud.
- Click the "circle arrow" icon on the ribbon (the left sidebar) **every time** you want to sync your vault between local and remote. (Or configure auto sync in the settings panel, see below.) While syncing, the icon becomes "two half-circle arrows". Besides clicking the icon, you can also activate the corresponding command in the command palette.
- **Be patient while syncing**, especially in the first-time sync.
- If you want to sync files across multiple devices, **your vault name should be the same** while using default settings.

### WebDAV

- Tutorials / Examples:
  - [Nextcloud](./docs/remote_services/webdav_nextcloud/README.md)
  - [The Good Cloud](./docs/remote_services/webdav_thegoodcloud/README.md)
  - [ownCloud](./docs/remote_services/webdav_owncloud/README.md)
  - [InfiniCloud](./docs/remote_services/webdav_infinicloud_teracloud/README.md)
  - [Synology webdav server](./docs/remote_services/webdav_synology_webdav_server/README.md) | [群晖 webdav server](./docs/remote_services/webdav_synology_webdav_server/README.zh-cn.md)
  - [dufs](./docs/remote_services/webdav_dufs/README.md)
  - [AList（中文）](./docs/remote_services/webdav_alist/README.zh-cn.md) | [AList (English)](./docs/remote_services/webdav_alist/README.md)
  - [坚果云](./docs/remote_services/webdav_jianguoyun/README.zh-cn.md) | [JianGuoYun/NutStore](./docs/remote_services/webdav_jianguoyun/README.md)
  - [Open Media Vault](./docs/remote_services/webdav_openmediavault/README.md)
  - [Nginx (`ngx_http_dav_module`, `nginx-dav-ext-module`, with Docker)](./docs/remote_services/webdav_nginx/README.md)
  - [Apache (with Docker)](./docs/remote_services/webdav_apache/README.md)
  - [Caddy with `http.handlers.webdav` module](./docs/remote_services/webdav_caddy/README.md)
- Very old versions of Obsidian need [configuring CORS](./docs/remote_services/webdav_general/webav_cors.md).
- Your data is synced to a `${vaultName}` sub folder on your webdav server.
- Password-based end-to-end encryption is also supported. But please be aware that **the vault name itself is not encrypted**.
- If you want to sync files across multiple devices, **your vault name should be the same** while using default settings.

### Dropbox

- **This plugin is NOT an official Dropbox product.** The plugin just uses Dropbox's public API.
- After the authorization, the plugin can read your name and email (which cannot be unselected), and read and write files in your Dropbox's `/Apps/remotely-save` folder.
- If you decide to authorize the plugin to connect to Dropbox, go to the plugin's settings, choose Dropbox, then follow the instructions.
- Password-based end-to-end encryption is also supported. But please be aware that **the vault name itself is not encrypted**.

### OneDrive for personal (App Folder)

- **This plugin is NOT an official Microsoft / OneDrive product.** The plugin just uses Microsoft's [OneDrive public API](https://docs.microsoft.com/en-us/onedrive/developer/rest-api).
- This plugin only works with "OneDrive for personal", and does not work with "OneDrive for Business" (yet). See [#11](https://github.com/fyears/remotely-save/issues/11) for details.
- After the authorization, the plugin can read your name and email, and read and write files in your OneDrive's `/Apps/remotely-save` folder.
- Password-based end-to-end encryption is also supported. But please be aware that **the vault name itself is not encrypted**.
- You might also want to checkout the [FAQ for OneDrive](./docs/remote_services/onedrive/README.md).

### Webdis

- Tutorial: [Webdis](./docs/remote_services/webdis/README.md)
- Mostly experimental.
- You have to setup and protect your web server by yourself.

## Smart Conflict (free now)

When a file is created or modified on both sides since the last sync, that's a conflict. The "Action For Conflict" setting in the settings panel decides what to do:

- **newer version survives** (default)
- **larger size version survives**
- **Smart Conflict (beta)**
  - For small markdown files, the plugin tries to merge the two versions with the diff3 algorithm.
  - For large files or non-markdown files, the plugin keeps both by renaming one of them.

This feature used to be a paid PRO feature of Remotely Save. In betterS3 it is available to everyone for free — just select it in the settings. **Please manually backup your vault before using it.**

## Scheduled Auto Sync

- You can configure auto syncing every N minutes in settings.
- In auto sync mode, if any error occurs, the plugin **fails silently**.
- Auto sync only works when Obsidian is open. It's **technically impossible** to auto sync while Obsidian is in the background, because the plugin just works in the browser environment provided by Obsidian.

## Sync On Save

- You can configure sync on save in settings.
- In sync on save mode, if any error occurs, the plugin **fails silently**.

## Config Folder / Files and Bookmarks

By default, the plugin doesn't sync config folder / files of Obsidian (normally `.obsidian` folder) because it's one of the hidden files.

However, you can enable syncing the config folder in settings. Please be aware that it's an experimental feature. Notably some config files' modified times are changed every time Obsidian is opened, and this destroys the assumptions of the plugin, so some information may not be synced properly.

Moreover, the bookmarks of Obsidian are actually stored in `.obsidian/bookmarks.json`. You can also set syncing this file (instead of the whole config folder) in settings. The plugin will keep the latest modified one across devices.

## Hidden Files Or Folders

**By default, all files or folders starting with `.` (dot) or `_` (underscore) are treated as hidden files, and would NOT be synced.** It's useful if you have some files just staying locally. But this strategy also means that themes / other plugins / settings of this plugin would neither be synced.

You can change the settings to allow syncing `_` files or folders, as well as the `.obsidian` special config folder as described above (but not any other `.` files or folders).

## How To Debug

If you see any errors, please check the doc [here](./docs/how_to_debug/README.md) for more details.

Moreover, sometimes the program runs but slowly, you may want to check the performance by [enabling the profiler](./docs/check_performance/README.md).

## Bonus: Import And Export Not-Oauth2 Plugin Settings By QR Code

See [here](./docs/import_export_some_settings.md) for more details.

## Acknowledgements

- betterS3 is forked from [Remotely Save](https://github.com/remotely-save/remotely-save), originally created by [fyears](https://github.com/fyears) and distributed under the [Apache License 2.0](./LICENSE). Thanks for the great work!
- Thanks to all the upstream contributors and the maintainers of the open source dependencies used by this plugin.

## License

[Apache License 2.0](./LICENSE).
