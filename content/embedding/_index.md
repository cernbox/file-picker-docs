---
title: 'Embedding'
weight: 10
type: docs
linkTitle: Embedding
date: 2022-04-01T13:29:48+02:00
---

To embed the File-picker, you have to use an `<iframe>` tag:

```html
<iframe
  src="https://filepicker.cernbox.cern.ch/?origin=https://mysite.cern.ch"
></iframe>
```

The source URL is https://filepicker.cernbox.cern.ch, and it must be passed an
`origin` query parameter containing the URL of the iframe parent.

> ⚠️ Remember, you have to specify the protocol (usually `https://`) in the origin.

This enables the iframe to check it against a list of approved sites before
setting it as `targetOrigin` argument of the `postMessage` the File-picker does.
Otherwise if the parent is vulnerable the iframe could be hijacked and the
message captured, including the user token.

By default anything under `cern.ch` is allowed. Outside sources can
[open an issue](https://github.com/cernbox/file-picker-wrapper/issues/new)
requesting addition to the whitelist.

## Operation modes

There are two ways to use the File-picker, depending on the value of the
`locationPicker` search parameter.

### File selector

This mode allows users to select files from their storage and then sends the
selection to the parent application. This selection can be sent as a URL or a
public link.

This is the default mode and there is no configuration setting for it.

#### URL sharing

By default, the File-picker will send the files' URL to the parent application,
along with an authentication token to enable the download. This token is usually
short lived, therefore this mode is more suited for downloading the selected
files straight away after the user has made the selection (for example to copy
them to the parent application's backend).

#### Public link sharing

Alternatively, the File-picker can generate public links for the selected files
and send them to the parent application. This can be enabled by passing the
`publicLink=1` parameter. These links will be valid in the longer term, and so
they can be used to link files somewhere without copying them. The duration of
the links can be adjusted with the `publicLinkDuration`, in days. These public
links will have the [`internal`](https://cs3org.github.io/cs3apis/#cs3.sharing.link.v1beta1.CreatePublicShareRequest)
flag set (so they are not visible in ownCloud web), and can be given a
description via the `description` parameter.

> ℹ️ Keep in mind, if the owner of the file deletes or moves it, the public link
> will stop being valid. This is especially important if you plan to have
> long-term access to the files.

### Location selector

This mode allows users to select a path inside their storage for the parent
application to upload files into it.

The File-picker will provide the parent application with authentication details
along with the selected path.

## Query parameters

The File-picker accepts the following parameters through the query string:

| Name           | Description                                                                                             | Values                    | Default | Required | Example                  |
| -------------- | ------------------------------------------------------------------------------------------------------- | ------------------------- | ------- | -------- | ------------------------ |
| origin         | URL of the iframe parent                                                                                | `string`                  |         | Yes      | `https://indico.cern.ch` |
| locationPicker | Show the location picker instead of the regular File-picker                                             | `bool`                    | `false` | No       | `1`                      |
| style          | Style to apply to the File-picker                                                                       | `light \| dark \| indico` | `light` | No       | `light`                  |
| userHome       | Whether the File-picker should start the browser in the user home folder instead of the root of CERNBox | `bool`                    | `true`  | No       | `0`                      |

The following parameters are only relevant when using the regular File-picker, not `locationPicker`.

| Name                  | Description                                                                                                                                                                              | Values   | Default | Required | Example                            |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- | -------- | ---------------------------------- |
| publicLink            | If `true`, the File-picker will generate [public links](https://doc.owncloud.com/webui/next/classic_ui/files/public_link_shares.html) for the shared files                               | `bool`   | `false` | No       | `1`                                |
| publicLinkDuration    | If `publicLink` is `true`, number of days links will be valid before expiry                                                                                                              | `number` | `0`     | No       | `30`                               |
| publicLinkDescription | [Description](https://cs3org.github.io/cs3apis/#cs3.sharing.link.v1beta1.CreatePublicShareRequest) for the public link                                                                   | `string` |         | No       | `CodiMD`                           |
| mimeTypes             | A url-encoded, comma-separated list of [MIME types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types) to filter the contents of the user storage | `string` |         | No       | `image%2Fjpeg%2Cimage%2Fsvg%2Bxml` |
