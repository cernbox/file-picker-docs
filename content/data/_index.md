---
title: 'Data'
weight: 20
type: docs
linkTitle: Data
date: 2022-04-01T13:29:48+02:00
---

## Message form

Whenever the user makes a selection on the File-picker, the iframe will post a
message to the parent with the following shape:

### When using basic sharing

Please refer to the [basic sharing](../embedding#basic-sharing) section for more
information.

```json
{
  "data": {
    "files": [
      "https://new.cernbox.cern.ch/remote.php/webdav/eos/user/j/johndoe/document_1.docx?access_token=123456789xyz",
      "https://new.cernbox.cern.ch/remote.php/webdav/eos/user/j/johndoe/document_2.docx?access_token=123456789xyz",
      "https://new.cernbox.cern.ch/remote.php/webdav/eos/user/j/johndoe/document_3.docx?access_token=123456789xyz",
      "https://new.cernbox.cern.ch/remote.php/webdav/eos/user/j/johndoe/document_4.docx?access_token=123456789xyz"
    ]
  }
}
```

### When using public link sharing

Please refer to the [public link sharing](../embedding#public-link-sharing)
section for more information.

```json
{
  "data": {
    "files": [
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/WLyTQEmXHJVlQdO/document_1.docx",
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/JgheoMVYenmfLGq/document_2.docx",
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/MvbhjhEJgjHassK/document_3.docx",
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/JBjteoIJgMJEQtz/document_4.docx"
    ]
  }
}
```

> **Notice:** any click in a file checkbox (select or deselect a file) will
> trigger a new message with the updated list of files. This is to so the users
> do not have to confirm their selection inside the File-picker.

This list of links can then be handled by the backend of the application using
the File-picker. They can be downloaded straight away and the token included as
parameter will be enough to authenticate the user.
