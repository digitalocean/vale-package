# DigitalOcean Vale Package

This repository contains a [Vale-compatible](https://vale.sh/) implementation of the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/).

## Getting Started

Add the package to your configuration file and run `vale sync`.

```ini
StylesPath = styles
Packages = https://github.com/digitalocean/vale-package/releases/latest/download/do-vale.zip

Vocab = DigitalOcean, Technical, TechProperNouns

[*.md]
BasedOnStyles = Vale, DigitalOcean, PDocs
```

See [Vale's documentation on packages](https://vale.sh/docs/topics/packages/) for more information.

### Integration with product-docs

The [product-docs repository](https://github.com/digitalocean/product-docs) uses this vale-package in its CI pipeline. The `product-docs/.vale.ini` references this package via the `Packages` directive. On each PR, GitHub Actions runs Vale, which downloads the latest package release and checks all modified Markdown files against the rules. Vale posts results as inline comments on the PR using reviewdog.

Repositories can disable specific rules in their local `.vale.ini` where there are valid exceptions. This architecture lets the vale-package be maintained independently and reused across multiple DigitalOcean documentation repositories.

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

Add new terms to the appropriate vocabulary file:

- **Product/brand names**: `styles/config/vocabularies/DigitalOcean/accept.txt`
- **General technical terms**: `styles/config/vocabularies/Technical/accept.txt`
- **Technical proper nouns**: `styles/config/vocabularies/TechProperNouns/accept.txt`
- **Compound words with valid alternatives**: `styles/ignore/words-with-suggestions.txt`

### Creating New Rules

To create a new rule:

1. Create a `.yml` file in `styles/DigitalOcean/` or `styles/PDocs/`.
2. Follow the rule structure pattern shown above.
3. Include a `link` to the relevant style guide section.
4. Test the rule against sample documentation.
5. Set the severity level based on the style guide.

### Testing Rules

Test rules locally by running Vale against sample documentation:

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

Each Vale rule corresponds to specific sections in the [DigitalOcean Style Guide](https://docs.digitalocean.com/style/). The `link` property in each rule file points to the relevant style guide page for reference.

## Repository-Specific Overrides

Repositories using this package can disable specific rules in their local `.vale.ini` when there are valid exceptions. For example, product-docs disables some rules that have valid use cases in support documentation:

```ini
PDocs.FirstPerson = NO
PDocs.Passive = NO
```

Document the reasoning when disabling rules.
