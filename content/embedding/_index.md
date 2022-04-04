---
title: "Embedding"
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

The source URL is https://filepicker.cernbox.cern.ch, and it must be passed an `origin` query parameter containing the
URL of the iframe parent.

This enables the iframe to check it against a list of approved sites before setting it as
`targetOrigin` argument of the `postMessage` the File-picker does.
Otherwise if the parent is vulnerable the iframe can be hijacked and the message captured including the user token.

By default anything under `cern.ch` is allowed. Outside sources can [open an issue](https://github.com/cernbox/file-picker-wrapper/issues/new)
requesting addition to the whitelist.
