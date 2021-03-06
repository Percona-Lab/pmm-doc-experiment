# pmm-doc-experiment

This is an experimental repo for evaluating [MkDocs](https://www.mkdocs.org/)
as an additional or replacement documention platform for the
PMM documentation repository ([pmm-doc](https://github.com/percona/pmm-doc)).

The markdown files were written by Sphinx using the
[Sphinx markdown builder](https://pypi.org/project/sphinx-markdown-builder/).

## Issues

- The source reST files have hard-wired references that cause broken links.
- Images rendered with `.. figure::` are missing and must be manually added.

## Installation

### Option 1: Docker

Use a [Docker](https://docs.docker.com/engine/install/) image with all prerequisites already installed:

```bash
# Clone this repository
cd pmm-doc-experiment
docker run --rm -it -p 8000:8000 -v $(pwd):/mkdocs pauljacobspercona/mkdocs-showcase mkdocs serve --dev-addr=0.0.0.0:8000
```

Open <http://localhost:8000> in a browser.

### Option 2: Native Install

#### Install Prerequisites (Debian/Ubuntu)

```bash
sudo apt install -y python3 python3-pip
```

(For other platforms, see <https://www.python.org/downloads/>.)

#### Check Prerequisites

```bash
python3 --version # >= 3.5
pip3 --version # >= 20.0.2
```

#### Install MkDocs and Extensions, Plugins, and Themes

```bash
pip3 install mkdocs
pip3 install mkdocs-exclude
pip3 install mkdocs-macros-plugin
pip3 install mkdocs-material
pip3 install mkdocs-redirects
pip3 install mkdocs-table-reader-plugin
pip3 install pymdown-extensions
pip3 install pygments
```

#### View Documentation

```bash
# clone this repository
cd pmm-doc-experiment
mkdocs serve
```

Open <http://localhost:8000>

#### Build documentation

To create a stand-alone static website:

```bash
mkdocs build
```

Open `site/index.html` in your browser.
