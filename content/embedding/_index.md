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
`publicLink` search parameter.

### Basic sharing

This mode which will add the user's authentication token to the files URLs. This
token is usually short lived, therefore this mode is more suited for downloading
the selected files straight away after the user has made the selection.

Basic sharing is the default and no configuration has to be set for it.

### Public link sharing

The second mode, which can be enabled by passing the `publicLink=1` parameter,
will generate public links for the selected files. These will be valid in the
longer term, and so they can be used to link files somewhere without copying
them. The duration of the links can be adjusted with the `publicLinkDuration`,
in days.

> ℹ️ Keep in mind, if the owner of the file deletes or moves it, the public link
> will stop being valid. This is especially important if you plan to have
> long-term access to the files.

## Query parameters

The File-picker accepts the following parameters through the query string:

| Name               | Description                                                                                                                                                | Values                    | Default | Required | Example                  |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | ------- | -------- | ------------------------ |
| origin             | URL of the iframe parent                                                                                                                                   | `string`                  |         | Yes      | `https://indico.cern.ch` |
| publicLink         | If `true`, the File-picker will generate [public links](https://doc.owncloud.com/webui/next/classic_ui/files/public_link_shares.html) for the shared files | `bool`                    | `false` | No       | `1`                      |
| publicLinkDuration | If `publicLink` is `true`, number of days links will be valid before expiry                                                                                | `number`                  | `7`     | No       | `30`                     |
| style              | Style to apply to the File-picker                                                                                                                          | `light \| dark \| indico` | `light` | No       | `light`                  |
| userHome           | Whether the File-picker should start the browser in the user home folder instead of the root of CERNBox                                                    | `bool`                    | `true`  | No       | `0`                      |
