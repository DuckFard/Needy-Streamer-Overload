# Linux DriftWM Port

This folder is a Linux-native DriftWM setup snapshot for the original
Needy Streamer Overload Rainmeter assets in this repository.

It is the setup used by `DuckFard/driftwm` for native Wayland NSO widgets.
The widgets are GTK/Cairo windows, not a localhost web dashboard or Firefox
tab. DriftWM window rules turn those toplevels into borderless movable canvas
widgets.

## Contents

- `scripts/nso_widget.py`: native GTK/Cairo widget implementation.
- `scripts/launch.sh`: launches the full widget suite.
- `config/default.toml`: runtime defaults.
- `config/nso.example.toml`: user override example for
  `~/.config/driftwm/nso.toml`.
- `config/driftwm.toml`: DriftWM autostart, keybinding, and window-rule
  snippet.
- `widgets/*/README.md`: widget-specific command notes.

## Dependencies

The DriftWM flake wraps these automatically. For manual runs you need:

- Python 3
- PyGObject / GTK 4
- Pycairo
- GdkPixbuf, Pango, PangoCairo introspection data
- `playerctl` for MPRIS media controls
- `xdg-open` / `gio` for desktop actions

## Run From This Fork

From the repository root:

```sh
python3 ports/linux-driftwm/scripts/nso_widget.py --widget welcome
```

Launch the full suite:

```sh
ports/linux-driftwm/scripts/launch.sh
```

List widget keys:

```sh
python3 ports/linux-driftwm/scripts/nso_widget.py --list
```

Known keys include:

```text
welcome task-manager ame jine social-media media-player calendar
desktop-icons quick-notes medications
```

## Installed DriftWM Setup

When packaged by `DuckFard/driftwm`, the commands are:

```sh
driftwm-nso
driftwm-nso-widget --widget quick-notes
```

On the target workstation, a convenience launcher may also exist:

```sh
nso
nso quick-notes
```

## DriftWM Config

Merge `config/driftwm.toml` into `~/.config/driftwm/config.toml`.

The important parts are:

```toml
autostart = ["driftwm-nso"]

[[window_rules]]
app_id = "dev.driftwm.nso.*"
decoration = "none"
border_width = 0
corner_radius = 0
shadow = false
```

After editing:

```sh
driftwm --check-config
driftwm msg action reload-config
```

Autostart changes apply on the next compositor restart. For the current
session, run `driftwm-nso` manually.

## User State

The DriftWM port writes user-editable state outside the repository:

```text
~/.config/driftwm/nso.toml
~/.local/state/driftwm-nso/state.json
~/.local/share/driftwm-nso/quick-notes.txt
```

Weather uses the original Calendar `Settings.inc` model. Put an
OpenWeatherMap city ID in `LocationCode`, an OpenWeatherMap key in `ApiKey`,
and set `Units` to `metric`, `imperial`, or `standard`.
