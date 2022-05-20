# Documentation

### Note: The RSS name is non-final, please don't exclusively bikeshed over it. It probably won't be called RSS.

The **RSS** (Robust Station Sound) format is intended to be a flexible, open, and readable way <!--Insert more marketing bull that sounds good here!--> to define various metadata such as licensing and attribution for a collection of sound files. An RSS is considered a set of sounds, and it can contain "tracks" which are single songs or sound effects within the RSS file. These tracks contain specific metadata such as the author of the track or a description of any modifications made to it (in compliance with many Creative Commons licenses).

An RSS is a folder with a name that ends in `.rss`, and contains a `meta.yaml` and one or more MIDI, OGG, WAV, or other supported files according to the names of the tracks.

The track metadata (what defines author, modifications, etc...) is stored in the `meta.json` file as JSON. The actual sprites are stored in sprite sheets as PNG files in the folder. Each unique state corresponds to a sprite sheet with the same name.

## YAML

The root of the YAML file contains the following values:

Key | Meaning
--- | -------
`version` | A simple integer corresponding to the RSS format version. This can be used to identify what version an RSS is and allow the implementation to correctly enable backwards compatibility modes when needed.
`license` | Optional. A valid [SPDX License Identifier](https://spdx.org/licenses/) applying to all tracks within this work.
`tracks` | A list of _tracks_ that store the actual meat of the RSS, see below.

### Tracks

A track is a container for metadata for a specific sound file. They store data like authorship and modifications performed on the track.

Tracks are distinguished by their key within the `tracks` list. This key is considered the name of the track.

Key | Meaning
--- | -------
`name` | The name of the track. Can only contain lowercase alphabetic, numerical, and some special (`_-`) characters. Does not include the file format of the track.

Tracks cannot have the same identifying value. Two tracks with the same name may not exist.

Other than the identifier, a track has other fields in relation to the actual tracks as seen in game:

Key | Meaning
--- | -------
`format` | The file format of the track file (such as `ogg`, `wav`, `midi`).
`channels` | A number corresponding to the amount of channels a track has. This should be a `1` for mono or `2` for stereo. Optional, defaulting to mono.
`flags` | An associative list of `key: object` for defining extra data. There is currently no usage yet. Optional.
`copyright` | The author or entity possessing copyright for the track. May also include a link to the source. Optional. Should be wrapped in double quotes (`""`) to make it clear that it's a string.
`modifications` | Many asset licenses require disclosure of modifications made to the file, and those can be described here. Optional. Should be wrapped in double quotes (`""`) to make it clear that it's a string.

Tracks are always ordered alphabetically by their corresponding file name.

### Example YAML

```yaml
version: 1
license: CC-BY-NC-SA-3.0
tracks:
- name: thunderdome
  format: ogg
  channels: 2
  copyright: "MashedByMachines. Source here: https://www.newgrounds.com/audio/listen/312622"
  modifications: "Coverted from MP3 to OGG and renamed thunderdome.ogg"
```

## Design Goals

* Editing an RSS must be possible without proper tooling. This means no binary metadata.
* It must be easily diffable on GitHub.
* It must not bloat Git history too much when changes are made (prevent large file rewrites).