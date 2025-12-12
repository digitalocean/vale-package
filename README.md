# DigitalOcean Vale Package

This repository contains a [Vale-compatible](https://vale.sh/) implementation of the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/).

## Getting Started

To get started, add the package to your configuration file (as shown below) and then run `vale sync`.

```ini
StylesPath = styles # Use your normal style path here.
Packages = https://github.com/digitalocean/vale-package/releases/latest/download/do-vale.zip
```

See [Vale's documentation on packages](https://vale.sh/docs/topics/packages/) for more information.

## Package Structure

The vale-package follows Vale's standard package structure and is organized as follows:

```
vale-package/
├── styles/
│   ├── DigitalOcean/       # Rules for general DigitalOcean style
│   ├── PDocs/              # Rules for Product Docs-specific style
│   ├── config/
│   │   └── vocabularies/   # Accepted terminology lists
│   │       ├── DigitalOcean/
│   │       ├── Technical/
│   │       └── TechProperNouns/
│   └── ignore/             # Words to ignore in spell checking
└── .vale.ini               # Configuration file
```

### Styles

#### DigitalOcean Style
Located in `styles/DigitalOcean/`, this style implements rules from the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/digitalocean/). Each `.yml` file represents a specific rule that checks for style violations.

**Key rules include:**
- **DigitalOceanTerms.yml** - Enforces correct capitalization and terminology for DigitalOcean products and services
- **Spelling.yml** - Spell checking with custom dictionary
- **SerialComma.yml** - Requires Oxford/serial commas
- **FutureTense.yml** - Discourages future tense in documentation
- **NumberCommas.yml** - Enforces comma formatting in numbers per Microsoft style guidelines
- **Units.yml** - Checks proper formatting of units of measurement
- **DateFormat.yml** - Enforces consistent date formatting

#### PDocs Style
Located in `styles/PDocs/`, this style implements additional rules specific to the [Product Documentation Style Guide](https://docs.digitalocean.com/style/pdocs/).

**Key rules include:**
- **Adverbs.yml** - Suggests removing unnecessary adverbs for clearer writing
- **Passive.yml** - Flags passive voice constructions
- **FirstPerson.yml** - Checks for inappropriate use of first person
- **Wordiness.yml** - Identifies wordy phrases with concise alternatives
- **ComplexWords.yml** - Suggests simpler alternatives to complex words

### Vocabularies

Vocabularies define accepted terms that should not be flagged as errors. Located in `styles/config/vocabularies/`:

- **DigitalOcean/** - DigitalOcean-specific product names, features, and terminology
- **Technical/** - General technical terms accepted in our documentation
- **TechProperNouns/** - Proper nouns for technical brands, products, and services

Vocabulary files use two lists:
- `accept.txt` - Terms accepted with this exact capitalization
- `reject.txt` - Terms that should never be used (often incorrect capitalizations)

### Ignore Lists

Located in `styles/ignore/`, these files contain words that should be ignored by spell checking:

- **words-with-suggestions.txt** - Common compound words and technical terms that have valid alternatives but are acceptable (e.g., "webpage", "webserver", "multicore")

These are separated from the main spelling checker because they have spelling suggestions but are valid in our documentation.

## Rule Structure

Each Vale rule file follows this general structure:

```yaml
extends: [rule_type]      # Base rule type (e.g., existence, substitution, spelling)
message: "..."            # Error message shown to users
link: https://...         # Link to relevant style guide section
level: error|warning|suggestion  # Severity level
# Additional rule-specific properties
tokens:                   # Patterns to match
  - pattern1
  - pattern2
```

### Common Rule Types

- **existence** - Checks for presence of specific words or patterns
- **substitution** - Suggests replacements using a swap map
- **spelling** - Spell checking against dictionaries
- **capitalization** - Enforces capitalization rules
- **occurrence** - Limits frequency of specific patterns

## Maintenance

### Adding New Terms

1. **Product/brand names** → Add to `styles/config/vocabularies/DigitalOcean/accept.txt`
2. **General technical terms** → Add to `styles/config/vocabularies/Technical/accept.txt`
3. **Technical proper nouns** → Add to `styles/config/vocabularies/TechProperNouns/accept.txt`
4. **Compound words with valid alternatives** → Add to `styles/ignore/words-with-suggestions.txt`

### Creating New Rules

1. Create a new `.yml` file in `styles/DigitalOcean/` or `styles/PDocs/`
2. Follow the rule structure pattern shown above
3. Include a `link` to the relevant style guide section
4. Test the rule against sample documentation
5. Set appropriate severity level based on the style guide

### Testing Rules

Test your rules locally by running Vale against sample documentation:

```bash
vale /path/to/test/file.md
```

## Included Components

* The `DigitalOcean` style implements the [DigitalOcean style guide](https://docs.digitalocean.com/style/digitalocean/).
* The `PDocs` style implements the [DigitalOcean Product Docs style guide](https://docs.digitalocean.com/style/pdocs/).
* The `DigitalOcean` vocab implements DigitalOcean-specific terminology.
* The `Technical` vocab implements more general technical terminology.
* The `TechProperNouns` vocab implements proper nouns for technical products and services.

## Style Guide Mapping

There are also a significant number of words defined in `.github/vale/ignore`, including notably a set of words with spelling suggestions which we want to catch with one of the package rules rather than with the less helpful spellchecker.

Each Vale rule corresponds to specific sections in the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/). The `link` property in each rule file points to the relevant style guide page for reference.
