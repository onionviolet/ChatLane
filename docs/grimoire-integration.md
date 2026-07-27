# Grimoire integration

This repository is an MIT-licensed derivative of
[RedMser/ChatLane](https://github.com/RedMser/ChatLane). It is maintained as
the conversion component used by [Grimoire](https://github.com/onionviolet/grimoire),
not as a replacement for the upstream ChatLane application.

## Boundary

Grimoire owns the visible chat-wheel experience:

- Selecting, editing, and validating chat-wheel configurations in its Electron UI.
- Choosing an installed Deadlock location and an add-on VPK destination.
- Installing, updating, disabling, and removing the generated add-on through its
  existing mod-management flows.

ChatLane owns the file-format conversion:

- Read a compatible ChatLane-generated VPK and recover its YAML configuration.
- Convert a valid YAML configuration into a compatible add-on VPK.
- Preserve the `chatlane.yml` entry so existing ChatLane add-ons continue to
  round-trip through Grimoire.

The Godot GUI is not embedded in Grimoire. This avoids shipping two unrelated
desktop interfaces while retaining the mature VPK generation implementation.

## Integration contract

The Grimoire host invokes the CLI with two positional paths:

```text
ChatLane.exe input.yml pakNN_dir.vpk
ChatLane.exe pakNN_dir.vpk output.yml
```

The host must treat a non-zero exit status as a conversion failure and display
the CLI's standard error/output to the user. Generated VPKs must be installed
as normal add-ons; the host must not silently alter `gameinfo.gi`.

## Attribution and updates

All distributed copies must retain the repository's `LICENSE`, including the
copyright notice for RedMser. Grimoire's credits and feature documentation
should link to both this fork and the original upstream repository.

Before adapting format-sensitive code, fetch `upstream` and review its release
notes and changes. Upstream fixes should be contributed back where they are
generally useful; Grimoire-specific host integration belongs in this fork or
the Grimoire repository.
