---
title: 'Setup'
weight: 100
type: docs
linkTitle: Setup
date: 2022-06-14T15:11:48+02:00
---

To set up your own File-picker instance, you need to do the following:

- Install the File-picker
- Edit the configuration files
- Set up a web server

This guide has been tested in CentOS 7 and Ubuntu 22.04, but it should work in
most distros, as it is a basic procedure.

> ℹ️ Note: The `rsync` package should be installed in the system.

# Install the File-picker

1. Clone the File-picker repo

```bash
mkdir -p /var/www/fp
cd /var/www/fp
git clone https://github.com/cernbox/file-picker-wrapper.git .
```

2. Run `make` to create the directory and files which will be served.

```bash
make dist
```

# Edit the configuration files

You can find templates of the configuration files in the repo.

- [`file-picker-config.json`](https://github.com/cernbox/file-picker-wrapper/blob/master/file-picker-config.json.template)
- [`allowed-origins.json`](https://github.com/cernbox/file-picker-wrapper/blob/master/allowed-origins.json.template)

The files should be placed in the `/dist` subdirectory created by `make` in the
previous step.

## file-picker-config.json

This file contains the server and authentication details:

```json
{
  "server": "https://example.com",
  "openIdConnect": {
    "metadata_url": "https://example.com/.well-known/openid-configuration",
    "authority": "https://example.com",
    "client_id": "example-client-id",
    "response_type": "code",
    "scope": "openid profile email",
    "redirect_uri": "https://example.com/oidc-callback.html",
    "silent_redirect_uri": "https://example.com/oidc-silent-callback.html",
    "logLevel": 1,
    "popupWindowFeatures": "location=no,toolbar=no,width=895,height=990,left=100,top=100"
  }
}
```

You want to configure the following fields:

- The hostname in `redirect_uri` and `silent_redirect_uri` should be the one where the file-picker
will be served from.
- `server` should be pointing to the owncloud instance.
  
Then, fill in the details for your OIDC provider in the `metadata_url`,
`authority` and `client_id` fields. These should point to the OCIS IDP or your
institution's SSO. You will have to register a `client_id` for the File-picker
in there. You can find more info in [OCIS documentation](https://doc.owncloud.com/ocis/next/deployment/services/s-list/idp.html)
or your institution's docs.

You can later adjust the `height` and `width` in `popupWindowFeatures` to
fine-tune the size of the login web dimensions.

## allowed-origins.json

This file contains a list of the origins that the file-picker will accept in the
[`origin` query parameter](docs/embedding/#query-parameters).
You can use the `*` wildcard to whitelist entire domains. Imagine you host two
applications in the `mycloud.net` domain which you want to allow use of the
File-picker:

- app1.mycloud.net
- app2.mycloud.net

To do so, you can set the contents of `allowed-origins.json` to:

```json
["*.mycloud.net"]
```

# Set up a web server

The File-picker requires a simple web server for the html and js bundle. This
example uses [nginx](https://www.nginx.com/). You will also need a proper SSL
certificate and key. Once you have those, first you enable access to the files
you created accessible for the `nginx` user:

```bash
cd /var/www/
chown -R nginx:nginx fp
```

Then, add the following to the `http` section of the `/etc/nginx/nginx.conf`.

> ℹ️ Note: Replace `YOURHOSTNAME` with the the hostname on which the File-picker
> will be available; and `YOURSSLCERTFILE` and `YOURSSLKEYFILE` with the path
> to the SSL certificate and key files.

```conf
[...]

server {
  listen 8080 default_server;
  listen [::]:8080 default_server;
  # hostname we will be serving on, e.g. localhost
  server_name YOURHOSTNAME;
  return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name YOURHOSTNAME;

  ssl_certificate YOURSSLCERTFILE;
  ssl_certificate_key YOURSSLKEYFILE;
  ssl_session_timeout 1d;
  ssl_session_cache shared:SSL:10m;

  ssl_protocols TLSv1.2;
  ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
  ssl_prefer_server_ciphers off;
  add_header Strict-Transport-Security "max-age=63072000" always;
  ssl_stapling on;
  ssl_stapling_verify on;

  # location of the build of file-picker
  root   /var/www/fp/dist;
  index  index.html;

  location / {
    try_files   $uri $uri/ =404;
    access_log  /var/log/nginx/fp.access.log;
    error_log   /var/log/nginx/fp.error.log;
  }
}
```

You can test the configuration with:

```bash
nginx -t
```

If the configuration is correct, restart nginx for the changes to take effect:

```bash
systemctl restart nginx
```

And that's all! You should be able to access the File-picker by navigating your
browser to the address you configured in nginx. Once you confirm it works, you
can embed it into an application by following the rest of these docs.

**Good luck! 🎉**
