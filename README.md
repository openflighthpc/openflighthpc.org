# Overview

A lightweight, easily-managed rebuild of the OpenFlightHPC project's history & existing projects. 

## Dev

- Run dev server on localhost (http://localhost:1313)
```bash
hugo server -F -D --cleanDestinationDir
```

## Release

To build the site simply
```bash
hugo --cleanDestinationDir
```
Then sync the contents of `public/` to the web root of a web server somewhere (probably want to remove any existing old/stuff so something like `rsync -auv --delete`)
