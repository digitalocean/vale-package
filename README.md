# DigitalOcean Vale Package

This repository contains a [Vale-compatible](https://vale.sh/) implementation of the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/).

## Getting Started

To get started, add the package to your configuration file (as shown below) and then run `vale sync`.

```ini
StylesPath = styles
MinAlertLevel = suggestion

Packages = DigitalOcean

[*]
BasedOnStyles = Vale, DigitalOcean
```

See [Vale's documentation on packages](https://vale.sh/docs/topics/packages/) for more information.
