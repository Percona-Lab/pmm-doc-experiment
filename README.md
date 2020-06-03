# pmm-doc-experiment

This is an experimental repo for evaluating [MkDocs](https://www.mkdocs.org/)
as an additional or replacement documention platform for the
PMM documentation repository ([pmm-doc](https://github.com/percona/pmm-doc)).

## How to Install and Run

### Install Prerequisites

Python: See <https://www.python.org/downloads/>

### Check Prerequisites

```bash
python3 --version # >= 3.5
pip3 --version # >= 20.0.2
```

### Install MkDocs and Extensions, Plugins, and Themes

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

### Clone this repository

```bash
git clone https://github.com/Percona-Lab/pmm-doc-experiment.git
```

### Run MkDocs

```bash
cd pmm-doc-experiment
mkdocs serve
```

### View the documentation

Open <http://localhost:8000>
