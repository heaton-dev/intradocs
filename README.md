# Intradocs | Hugo + Docsy Documentation Generator

Ready to go technical documentation powered by Markdown.

## What is Hugo?

[Hugo](https://gohugo.io/) is a [Static Site Generator](https://www.cloudflare.com/en-gb/learning/performance/static-site-generator/) written in Go which parses a directory and file hierarchy of your content to produce a templated website.

Hugo's capabilities include:

- Blazing fast processing times, <1ms per page
- Go templating language
  - Use shortcodes for custom behaviour
- Markdown
  - Best markup solution for creating documents
  - Lots of tools, validation, extensions and GUIs available

## What is Docsy?

Docsy is a Hugo theme orientated towards technical documentation, find the source at [github.com/google/docsy](https://github.com/google/docsy/). The project is not officially supported by Google but is actively maintained.

We use Docsy to reduce the time to deploy documentation, and to benefit from upstream development.

Docsy is used by:

- [Kubernetes](https://kubernetes.io/)
- [Knative](https://knative.dev/) Serverless Kubernetes
- [Flux](https://fluxcd.io) Kubernetes GitOps
- [GRPC](https://grpc.io/) CNCF project
- [Agones](https://agones.dev/site/) Game servers on K8S

This repo is based on [google/docsy-example](https://github.com/google/docsy-example).

## Why this solution over *your favourite WYSIWYG editor*

**As software developers** we want to include information about our software project in the same place as the code, typically this is a README.md but when we want to start splitting our instructions into multiple pages we want the ability to generate a self-contained documentation website that we can host uses GitLab/GitHub Pages services, or display locally.

**As product developers** we want to develop our developer and user documentation alongside the product, iterating on the instructions as features evolve. Docsy supports versioning, so when we tag a new release we can generate a separate documentation version, accessibile from the same single documentation website.

**As a WYSIWYG user** you keep the flexibility (embed, images, formatting..) but broaden the capabilities of your team's development pipeline by using Git. Documentation via Git enables collaborative review, discussion and tracking of changes.

## Project aims

This project is a proof-of-concept.

***Quick* and *easy* documentation via Git.**

## Requirements

- [Hugo](https://gohugo.io/) >= 0.73.0
  - Extended version (with bundled SCSS support)

Hugo is available via [docker.io/r/klakegg/hugo](https://hub.docker.com/r/klakegg/hugo/), use `ext-alpine` tag for the Extended version.

## Usage

### Build

Run hugo in a container:

```shell
cd docsy
docker run -v $(pwd):/src -p 1313:1313 -it docker.io/klakegg/hugo:ext-alpine server -w --buildDrafts --buildFuture --config config.toml
```

Or, install hugo locally using specific a [release](https://github.com/gohugoio/hugo/releases).

## Content

Hugo sources content from a directory hierarchy of Markdown/HTML files stored in `content/`.

All pages and folders require a `_index.md` file which describes the page.

Example section `_index.md`:

```yaml
---
title: "Engineering Dept."  # Page title
linkTitle: "Eng Dept."      # Override title in menu
description: |              # Shown at top of page and in search results
  The team that gets things done
tags: [engineering]         # Used for search
icon: "fas fa-cogs"         # Prefix an icon to the sidebar menu item, uses FontAwesome class names
---

My section content
```

Example page `index.md`:

```yaml
---
title: "My Team"          # Page title
linkTitle: "My Team"      # Override title in menu
description: |            # Shown at top of page and in search results
  The team that gets things done
tags: [engineering]       # Used for search
icon: "fas fa-cogs"       # Prefix an icon to the sidebar menu item, uses FontAwesome class names
---

My page content
```

Example content hierarchy:

```bash
content/  # Top-level folder
└── en    # Language folder, used even in single-language sites
    ├── blog            # Docsy supports blog-style news sites
    │   ├── _index.md   # All sections require a _index.md that contains their metadata
    │   ├── news            # Blog section
    │   │   ├── first-post  # Each page gets its own folder
    │   │   │   ├── assets  # first-post assets; i.e. images, pdf, tar..
    │   │   │   │   └── featured-sunset-get.png
    │   │   │   └── index.md  # first-post page content
    │   │   ├── _index.md
    ├── community           # Page to share relevant links for your org/project
    ├── docs                # Root folder for all documentation
    │   └── Engineering Teams
    │       ├── Platform Team
    │       │   ├── Guides
    │       │   │   └── _index.md   # Guide page
    │       │   ├── Standards
    │       │   │   └── _index.md   # Standards page
    │       │   └── _index.md       # Platform Team page
    │       └── _index.md           # Engineering Teams page
    ├── _index.html   # Homepage
    └── search.md     # Meta-file to support offline search
```

### Create a section

Sections can contain more sections and pages inside them.

```shell
mkdir content/en/docs/SECTION
cat <<-EOF > content/en/docs/SECTION/_index.md
---
title: "Engineering Teams"
linkTitle: "Engineering Teams"
description: |
  The team that gets things done
tags: [engineering]
icon: "fas fa-cogs"
---

EOF
```

### Create a leaf page

This is the simplest kind of page, with no nested pages beneath it.

```shell
mkdir content/en/docs/SECTION/PAGE
cat <<-EOF > content/en/docs/SECTION/PAGE/index.md
---
title: "My Teams"
linkTitle: "My Teams"
description: ""
---

EOF
```


## Update Hugo version

Hugo's version is set in `.github/workflows/pages.yaml` and `gitlab-ci.yml`.

Ensure you use the `hugo_extended` image, which enables hugo to compile SCSS.

## Pipeline

The included GitHub pipeline sets up production dependencies, such as NodeJS for augmenting the CSS with better browser compatibility, and deploys the main branch to GitHub Pages.
