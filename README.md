# Intradocs

Ready to go technical documentation.

Based on [google/docsy-example](https://github.com/google/docsy-example) which uses [Hugo](https://gohugo.io/) static site generator.

## Develop

Run the `hugo` command locally to build the website into a directory, there are flags to enable building pages with `draft: true` or pages that are dated for future release, etc..

By running `hugo server -w` hugo will host the website on a local webserver at `http://localhost:1313` and update automatically (including refreshing your browser) when changes are detected.

```shell
hugo server -w --buildDrafts --buildFuture --config config.toml
```

First start-up will take time as hugo downloads dependencies. See [Hugo Modules](https://gohugo.io/hugo-modules/).

## Publish

The recommended way to publish Hugo websites is to enable the deployment pipeline for your Git platform, config files are included for GitHub & GitLab.

The pipeline sets up production dependencies, such as NodeJS for augmenting the CSS with better browser compatibility.

## Update Hugo version

Hugo's version is set in `.github/workflows/pages.yaml` and `gitlab-ci.yml`.

Ensure you use the `hugo_extended` image, which enables hugo to compile SCSS.
