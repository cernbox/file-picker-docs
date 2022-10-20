---
title: 'Documentation'
weight: 1
type: docs
linkTitle: Documentation
date: 2022-04-01T13:29:48+02:00
description: >
  How to use the CERNBox File-picker in your site.
---

Welcome to the File-picker docs.

## What does the File-picker do?

The CERNBox File-picker provides a way to integrate a cloud storage into other
applications.

It can be embedded in a web, and will show a file browser which allows the users
to navigate their CERNBox storage.

The File-picker features two use cases:

- **Select files** from the user storage and make them available to the parent
  application via their URL a public link.
- **Select a location** from the user storage and pass it to the parent
  application for it to upload files into.

Once the user has made a selection, it will send the parent application URLs or
public links for the selected files.

The File-picker handles authentication into CERNBox itself, and in the case of
using file URLs, those include a token to grant access for the parent
application. When using public links this is not needed.

## How to use it

Here is a small guide to integrate CERNBox in a site using the File-picker.

- How to [embed the file-picker in your site](embedding), different modes of
  operation and configuration parameters.
- How to [get files from CERNBox](data) once the user has selected some.
- How to [style the file-picker](style).

## How to set up your own

- [Setting up](setup) a File-picker for your ownCloud instance.
