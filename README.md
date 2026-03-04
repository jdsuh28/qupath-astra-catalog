# Template for catalogs of QuPath extensions

This repo aims to make it easy to set up a catalog of QuPath extensions. It provides a basic example catalog and a github action to validate the catalog.

## Setup

First click `Use this template -> Create a new repository` on GitHub, and clone the new repository to your computer using git or https.

With git:

```bash
git clone git@github.com:my-user/my-catalog-repo.git
```

Next, set up either a virtual environment or a conda environment and activate it.

To set up and activate a virtual environment:

```bash
python -m venv env
. env/bin/activate
```

To set up and activate a conda environment:

```bash
conda create -n env python=3.10
conda activate env
```

Then, install the extension-catalog-model package into the conda or virtual environment:

```bash
pip install git+https://github.com/qupath/extension-catalog-model
```

## Creating and validating catalogs

To create a model in python:

```python
from extension_catalog_model.model import *

version_range = VersionRange(min="v0.5.1")
release = Release(
    name="v0.1.0-rc5",
    main_url="https://github.com/qupath/qupath-extension-omero/releases/download/v0.1.0-rc5/qupath-extension-omero-0.1.0-rc5.jar",
    optional_dependency_urls=["https://github.com/ome/openmicroscopy/releases/download/v5.6.14/OMERO.java-5.6.14-ice36.zip"],
    version_range=version_range
)
extension = Extension(
    name="QuPath OMERO extension",
    description="QuPath extension to work with images through OMERO's APIs",
    author="QuPath",
    homepage="https://github.com/qupath/qupath-extension-omero",
    releases=[release]
)
catalog = Catalog(
    name="QuPath catalog",
    description="Extensions maintained by the QuPath team",
    extensions=[extension]
)

# write the catalog to a file
with open("catalog.json", "w") as f:
    f.write(catalog.model_dump_json(indent=4))
```

This will create a Python object representing the catalog and the extensions it contains, and then write that to a `catalog.json` file in your current directory.

Alternatively, you can directly edit the JSON file according to the JSON schema [defined in the `extension-catalog-model` package](https://qupath.github.io/extension-catalog-model/autodocs.html).

To validate a catalog in python:

```python
from extension_catalog_model import Catalog
with open('catalog.json') as f:
    jsontext = f.read()
    Catalog.model_validate_json(jsontext)
```

If this runs without errors, then your `catalog.json` is valid and should work! If not, hopefully the errors are informative enough to explain where you went wrong. If not, raise an issue on [the forum](https://forum.image.sc/) or [on GitHub](https://github.com/qupath/extension-catalog-model/issues)


## Updating the catalog

To update the catalog, use git to add and commit the changes, and then push the changes to GitHub.

```bash
git add catalog.json
git commit -m "Update catalog"
git push
```
