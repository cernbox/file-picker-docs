---
title: 'Data'
weight: 20
type: docs
linkTitle: Data
date: 2022-04-01T13:29:48+02:00
---

The message posted to the parent will always include two fields: a `ready`
boolean and a `files` array of strings. This is valid for all working modes.

The File-picker works asynchronously. It has to make some requests to validate
the token used to access the ownCloud instance and, when sharing public links,
it also has to wait for the link generation.

Whenever the user makes a selection on the File-picker, the iframe will post an
instant message to the parent with the `ready` boolean set to `false` and an
empty `files` array. This can be used by the application to disable the
confirmation button or show a loading spinner. Once the work is done, another
message will be posted, with `ready` set to `true` and the list of URLs for
selected files.

> **Notice:** any click in a file checkbox (select or deselect a file) will
> trigger a new message with the updated list of files. This is to so the users
> do not have to confirm their selection inside the File-picker.

This list of links can then be handled by the backend of the application using
the File-picker. They can be downloaded straight away and the token included as
parameter will be enough to authenticate the user.

## File selector

### When using URL sharing

Please refer to the [URL sharing](../embedding#url-sharing) section for more
information.

```json
{
  "data": {
    "ready": true,
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
    "ready": true,
    "files": [
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/WLyTQEmXHJVlQdO/document_1.docx",
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/JgheoMVYenmfLGq/document_2.docx",
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/MvbhjhEJgjHassK/document_3.docx",
      "https://newqa.cernbox.cern.ch/remote.php/dav/public-files/JBjteoIJgMJEQtz/document_4.docx"
    ]
  }
}
```

## Location selector

Please refer to the [location selector](../embedding#location-selector)
section for more information.

```json
{
  "data": {
    "ready": true,
    "files": ["/example/folder/under_user_home"],
    "username": "johndoe",
    "accessToken": "123456789xyz"
  }
}
```
