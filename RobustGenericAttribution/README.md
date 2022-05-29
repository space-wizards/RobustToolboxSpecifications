# Documentation

The **RGA** (Robust Generic Attribution) standard is intended to be a flexible, open, and readable way <!--Insert more marketing bull that sounds good here!--> to define various metadata such as licensing and attribution for an arbitrary collection of files of many types such as sound effects or prototypes. An RGA file contains metadata for all of the files in the same directory as the RGA file (not including subdirectories). The entries in an RGA file contain specific metadata such as the author of the file(s) or a description of any modifications made to them (in compliance with many Creative Commons licenses).

An RGA is a YAML file with the name `attributions.yml`, and contains an arbitrary number of entries as defined below.

Typically whenever new files are being added, a new entry will be added as it likely won't contain the same metadata as existing entries. However, one may append files to an existing entry if the metadata is otherwise identical.

## YAML

An RGA file must be named `attributions.yml`. All values within entries are wrapped in double-quotes (`""`).

The YAML contains an arbitrary number of entries, covering all files in the same directory as the RGA file. An entry is defined as follows:

Key | Meaning
--- | -------
`files` | An array of filenames (with extensions) that this entry applies to. The filename order is arbitrary.
`copyright` | The copyright holder and info for finding the source of the file(s).
`license` | A valid [SPDX License Identifier](https://spdx.org/licenses/) applying to all files within an entry.
`modifications` | Optional. Disclosure of any modifications to comply with applicable licenses.

### Example YAML

```yaml
- files: [ "metal1.ogg", "punch.ogg", "zap3.ogg" ]
  license: "CC-BY-SA-3.0"
  copyright: "https://github.com/tgstation/tgstation at commit 583efc098b3ce871715afd02d0f9990150a48ec2"
  modifications: "Converted to OGG and compressed."
- files: [ "mind_crawler.ogg" ]
  license: "CC-BY-SA-4.0"
  copyright: "https://github.com/BeeStation/BeeStation-Hornet at commit 10220eb0c787e47e8f845a11b43ed140bbf72889"
```

## Design Goals

* Editing an RGA must be possible without proper tooling. This means no binary metadata.
* It must be easily diffable on GitHub.
* It must not bloat Git history too much when changes are made (prevent large file rewrites).