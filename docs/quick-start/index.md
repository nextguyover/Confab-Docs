# Quick Start Guide

This page will outline the fastest way to get Confab up and running on your website.

## What You Need

Confab backend requires a server to run on. Most OSs and architectures are supported(1); we provide native builds and scripts for Debian-based linux distributions. Additionally, you will need some way to execute commands on your server, e.g. SSH.
{ .annotate }

1.  Confab backend is written in ASP.NET Core, and can run on Linux, Windows, macOS, and more.

You will also require SMTP credentials from an email provider. This is a mandatory requirement, as user authentication happens over email.

## Installation

=== "Auto-install Script"

    **Natively supported OS only (`linux-x64`)**

    1. `cd` to the directory that you want to install Confab (subfolder `./Confab` will be created automatically)
    2. Copy and paste the code below into your terminal(1)
        { .annotate }

        1.  Executing unknown scripts from the internet like this is bad practice, we recommend that you read [the script below](https://github.com/{{variables.CONFAB_GITHUB_LOCATION}}/blob/prod/scripts/autoinstall.sh){:target="_blank"} yourself and verify that it does what you want

    ```bash
    sudo bash -c "$(curl https://raw.githubusercontent.com/{{variables.CONFAB_GITHUB_LOCATION}}/prod/scripts/autoinstall.sh)"
    ``` 
=== "Manual Install"

    **Natively supported OS only (check GitHub releases)**

    1. `cd` to the directory that you want to install Confab
    2. Download the release for your operating system from the [releases section of the Github repo](https://github.com/{{variables.CONFAB_GITHUB_LOCATION}}/releases/){:target="_blank"}
    3. Extract the archive to your install location
=== "Manual Compile"

    **Any OS**

    Follow the instructions to [build Confab yourself](../development/index.md#building-confab).


## Server Setup

### Configuration

Open the `appsettings.json`(1) file with your favourite text editor. This is the Confab server configuration file. [Many configuration options](../config/index.md) are available, we will outline the minimal settings you need to get started.
{ .annotate }

1. Same directory as your Confab executable. If you used the install script or did a manual install, this will be located at `Confab/appsettings.json`. If you compiled Confab yourself, this will be at `Confab/App/appsettings.json` 

Firstly, edit the `ConfabParams` section

- Set `ConfabParams:ExternalUrl` to the URL that this server will be hosted at
- Add a value to `ConfabParams:CommentsAtLocation` with the URL of the site that you will be embedding Confab into

Next, edit the `Emails` section

- Set `Emails:SMTP:Server` to your SMTP server
- Set `Emails:SMTP:Port` to your SMTP port
- Set `Emails:SMTP:UseTLS` to `true` if you're using TLS for your SMTP connection (highly recommended)

Next, set the `Emails:TemplateParameters` section. These are the values that will be substituted into the emails sent by your server.

- Set `Emails:TemplateParameters:ServiceName` to the name of your site
- Set `Emails:TemplateParameters:SiteUrl` to the URL of your site 
- Set `Emails:TemplateParameters:ConfabBackendApiUrl` to the URL that this server will be hosted at

Almost there! Set the `Emails:SendingAddresses`, each of the subcategories can be configured to use a sending address of your choice.

- Set `Emails:SendingAddresses:<category>:Address` to the sending address
- Set `Emails:SendingAddresses:<category>:Username` to the SMTP username
- Set `Emails:SendingAddresses:<category>:Password` to the SMTP password

Finally, scroll down to `UserRoles:Admin`, and add a string with your email address (logging in with this email will give you admin privileges).

### Starting Confab

Confab can be started by launching the `Confab` executable. To run Confab at system startup, a bash script has been provided to install Confab as a systemd service. Run the script with the following command.


```bash
sudo bash autostart-linux.bash --enable # (1)!
```
{ .annotate }

1.  CLI Options:
```
Enable or disabled Confab autostart via systemd service. Confab excecutable must be in the same directory as this script.
Usage: autostart-linux.bash [--enable|--disable]

This script must be run with elevated privileges (sudo)
```

This will create the systemd service, start it, and provide instructions on how to optionally monitor the log output.

### Exposing to Internet

At this stage, Confab should successfully be running on your system (default port `2632`). 

Confab does not have any built-in functionality for HTTPS, so it is recommended you use a reverse proxy such as [Caddy](https://caddyserver.com/docs/quick-starts/reverse-proxy){:target="_blank"} or [NGINX](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/){:target="_blank"}, or something like [Cloudflare Tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-local-tunnel/){:target="_blank"} to allow access to Confab on your chosen domain securely over SSL.

!!! danger "Use HTTPS"

    It is highly recommended that you do **not** expose Confab to the internet without HTTPS

## Site Integration

Embedding Confab onto your site is as simple as adding a `<script>` tag at the position where you want the Confab widget to appear.

```html
<script src="https://comments.example.com/confab-loader.js"></script> <!-- (1)! -->
```
{ .annotate }

1. Replace `comments.example.com` with the domain that your Confab backend is hosted at

## Enabling Commenting

To enable comments at a given location on your site, open your site at that page. Scrolling to the Confab widget, you will see an error message (this is because this location has not yet been enabled). (1)
{ .annotate }

1. Read more about [how comment locations work](../core-functionality/location/index.md)

Click "Go to login" on the top-right corner of the widget, and sign in with your Admin email. Once you've logged in, open the [Admin Panel](../core-functionality/admin-panel/index.md#commenting-settings), then initialise that location.

## Next Steps

Congratulations! 🎉 At this stage, you should have Confab working on your website. 

As an Administrator, there are a few things that you should know. Hosting user generated content on your site can come with risks, so Confab provides tools such as [Automatic Moderation](../core-functionality/auto-moderation/index.md) and a [Manual Moderation Queue](../core-functionality/manual-moderation/index.md) to allow screening of user comments before they become visible on your site. 

Furthermore, you should be aware of the [possible risks of allowing images](../admin-guide/content-risks/index.md#images) on your site, and [how images can be disabled](../admin-guide/content-risks/index.md#blocking-images).

!!! tip "Admin Tips"
    Read more about these issues, and other info you should know as an admin, by checking out the [Admin Guide](../admin-guide/index.md).

