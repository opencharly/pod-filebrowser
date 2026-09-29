# pod-filebrowser

The `filebrowser` candy of the OpenCharly candy library, as a standalone repo
(kind-prefixed naming). It ships [FileBrowser Quantum](https://github.com/gtsteffaniak/filebrowser)
as a supervised web file manager.

## What it provides

Downloads the pinned `gtsteffaniak/filebrowser` static binary
(`${FILEBROWSER_VERSION}`, default `v1.2.4-stable`) to
`/usr/local/bin/filebrowser`, writes its server config to
`/etc/filebrowser/config.yaml`, and runs it as the always-restart `filebrowser`
service. The `data` volume persists the SQLite database and cache; the `files`
volume is the user-browsable tree.

| Property | Value |
|---|---|
| Service | `filebrowser` (`/usr/local/bin/filebrowser`, `restart: always`) |
| Port | `8080` (web UI) |
| Requires | `layer-supervisord` |
| Env | `FILEBROWSER_CONFIG=/etc/filebrowser/config.yaml` |
| Volumes | `data` at `~/.filebrowser/data`, `files` at `~/.filebrowser/files` |
| Alias | `filebrowser` (runs in the container) |
| Config | `config.yaml` → `/etc/filebrowser/config.yaml` |

## How to use it

Compose the candy into a box, then bind the `files` volume to the host tree you
want to browse:

```bash
charly box build <box-composing-filebrowser>
charly config <box> --bind files=~/Documents
charly start <box>
# open http://localhost:8080
```

The `files` volume can be bind-mounted to a home directory, a NAS mount, or a
project directory; the `data` volume keeps the database across updates.

## Layout

- `charly.yml` — the `filebrowser:` candy entity plus its `skill:` entity.
- `config.yaml` — the server config staged to `/etc/filebrowser/config.yaml`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-filebrowser:filebrowser` — the service properties, the
  config, the volumes, and verification.
- `/charly-infrastructure:supervisord` — the process-manager dependency.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
