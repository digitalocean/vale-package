# DigitalOcean Vale Package

This repository contains a [Vale-compatible](https://vale.sh/) implementation of the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/).

## Getting Started

To get started, add the package to your configuration file (as shown below) and then run `vale sync`.

```ini
StylesPath = styles # Use your normal style path here.
Packages = https://github.com/digitalocean/vale-package/releases/latest/download/do-vale.zip
```

See [Vale's documentation on packages](https://vale.sh/docs/topics/packages/) for more information.

## Included Components

* The `DigitalOcean` style implements the universal DigitalOcean style guide.
* The `PDocs` style implements the DigitalOcean Product Docs style guide.
* The `DigitalOcean` vocab implements DigitalOcean-specific terminology.
* The `Technical` vocab implements more general technical terminology.
