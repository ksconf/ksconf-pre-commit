# KSCONF Pre-commit hooks

This repo provides a simple [pre-commit](https://pre-commit.com/) hook definition for the [ksconf](https://github.com/ksconf/ksconf) project.

## Hooks

| Hook id | Description |
| ------- | ----------- |
| `ksconf-check` | Perform basic validation tests against all files in your repo that end with `.conf` or `.meta` |
| `ksconf-sort` | Normalize `.conf` and `.meta` files, making diffs more readable and merging more predictable |
| `ksconf-xml-format` | Consistently format Simple XML dashboards, Dashboard Studio (embedded JSON), and navigation files |

## Example Usage

`.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/ksconf/ksconf-pre-commit
    rev: v0.13.10
    hooks:
      - id: ksconf-check
      - id: ksconf-sort
        exclude: logging\.conf$
      - id: ksconf-xml-format
```

For additional information, see [ksconf pre-commit hooks](https://ksconf.readthedocs.io/en/stable/git.html#pre-commit-hooks) in the official ksconf documentation.


## Background

Pre-commit hooks used to be directly supported by the main [ksconf](https://github.com/ksconf/ksconf) repo.
However, this lead to some complex dependency processing as the same repo housed a Python package, the Splunk app, and pre-commit hooks.
Specifically, the `lxml` library, used by the xml formatting functionality, needs to be present for the pre-commit hook,
but must be absent when packaged into a Splunk app, to avoid platform dependencies.
This ended up causing confusion, hard dependencies when "extras" would have been good enough, and lots of frustration when dependencies change.
