# Valu Site Manager

## Important:

Version 1.0

This bash script makes it easy to spin up a new WordPress site using [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV) version 2.

## Installation

Download the script and run `./vsm` from the directory the script is placed in.

To run the script from anywhere, you can place `vsm` in a folder included in your PATH environment variable.

If you don't want to define the path each time you run the script, open the file and uncomment the line at the top defining `path`. Set that to the root folder of VVV on your machine. **Note:** You do not need to do this if VVV is installed in the default location (`~/vagrant-local/`).

## Usage

Type `vsm` in the command line to use. None of the options are required: if a required piece of information is not included in the original command, the wizard will prompt you for it.

### Options

|Option|Name       |Description|
|------|-----------|-----------|
|`-a`  |action     |`new`/`create`/`make` runs the site creation wizard, `rm`/`delete`/`teardown` runs the site teardown wizard, `list` lists all current VVV sites|
|`-d`  |domain     |Desired domain (e.g. mysite.dev)|
|`-f`  |files only |Do not provision Vagrant, just create the site directory and files|
|`-i`  |image proxy|Load images by proxy from the live site (so you don't have to download the uploads folder)|
|`-l`  |live URL   |URL of the live site, currently only used if loading images by proxy|
|`-n`  |site name  |Desired name for the site directory (e.g. mysite)|
|`-p`  |path       |Path to VVV root (e.g. ~/vagrant-local)|
|`-v`  |version    |Version of WordPress to install|
|`-x`  |debug      |Turn on WP_DEBUG and WP_DEBUG_LOG|

Examples:

```
vsm -a new
vsm -a create -n mysite -d mysite.dev -v 3.9.1 -x
vsm -a make -fxil mysite.com -n mysite -d mysite.dev -v 3.9.1
vsm -a delete mysite
vsm -a list
```

## Site Creation Wizard

Creating a site does the following:

* Halts Vagrant (if running)
* Creates a web root for the site in the `www` folder and `wp-cli.yml` file inside it. In addition script creates a provision folder containing three files: `vvv-init.sh` and `vvv-hosts` and `vvv-nginx.conf`
	* `wp-cli.yml` tells WP-CLI that WordPress is in the public_html folder
	* `vvv-init.sh` tells Vagrant to create a database if one does not exist and install the latest version of WordPress (via WP-CLI) the next time Vagrant is provisioned
	* `vvv-hosts` contains the hosts entry to give your site a nice custom domain (the domain is set in the wizard)
	* `vvv-nginx.conf` to handle server settings for your site
* Restarts Vagrant with `vagrant up --provision`

Provisioning Vagrant takes a couple of minutes, but this is a crucial step as it downloads WordPress into your site's public_html directory and runs the installation. If you want to skip provisioning and install WordPress manually, you can run the new site's `vvv-init.sh` file directly in the Vagrant shell.

## Site Teardown Wizard

Deleting a site does the following:

* Halts Vagrant (if running)
* Deletes the site's web root

Note that it does not delete the site's database.

## Changelog

### 1.0

* Initial release for VVV 2.0
