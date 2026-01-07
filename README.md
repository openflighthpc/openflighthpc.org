# Overview

A lightweight, easily-managed rebuild of the OpenFlightHPC project's history & existing projects. 

## Content

There are 2 types of content for the website

### Labs

Lab content is a single blog post that describes the purpose and understanding of a tool we've experimented with. 

Create a lab element under `content/labs/` and probably include the following frontmatter:
```yaml
---
title: My Experiment
description: Experimenting with my things
date: 2025-12-25
params:
  status: active
  source: https://example.com
---
```

Probably make `date` reflective of when the experiment was done and `status` either `active` or `archived`.

### Knowledge

Knowledge content are categorised guides/documentation.

Create a parent folder reflective of the category/series that documentation relates to and then the file, such as `content/knowledge/my-series/first-page.md` and probably include the following frontmatter:
```yaml
---
title: First Page
date: 2023-01-01
categories:
- My Series
---
```

Category is needed otherwise it won't appear in any of the website nav.

To link to other pages within the website sensibly use the shortcode helper in markdown:
```markdown
[LINK_TEXT]({{< ref "path/to/markdown-file-without-extension" >}}) 
```

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
