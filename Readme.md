# workshop-docs

## Requirements

* [mkdocs](http://mkdocs.org/)
* [Markdown-Knowledge](https://daringfireball.net/projects/markdown/)

## Usage

1. create your workshop-documentation as one or multiple files under `docs`
1. modify `docs/index.md` to reference satisfactory to your documentation
1. modify `mkdocs.yml#pages` to reference satisfactory to your documentation

## build static files (for deployment)

    mkdocs build

### serve documentation locally (for live review)

    mkdocs serve

### deploy to github

    mkdocs gh-deploy