# Godot Installer

Windows installers for Godot Engine 3 and 4, built with Inno Setup for lab machines at Instituto Federal de Mato Grosso.

[![Build Installers](https://github.com/carlosrabelo/godot-installer/actions/workflows/build.yml/badge.svg)](https://github.com/carlosrabelo/godot-installer/actions/workflows/build.yml)
[![Release](https://img.shields.io/github/release/carlosrabelo/godot-installer.svg)](https://github.com/carlosrabelo/godot-installer/releases)

## Highlights

- Build Windows installers for Godot 3 and Godot 4 with Inno Setup
- Install or update Godot on lab machines with a single click
- Bump engine versions with a one-line edit in each `Godot.iss` file
- Rebuild only the Godot line whose `Godot.iss` changed on GitHub Actions
- Publish a per-line GitHub Release (`godot3-*` / `godot4-*`) on push to `master`
- Keep Godot setups consistent across computer lab machines

## Overview

This project packages the official Godot Engine Windows binaries into Inno Setup installers so students and lab staff can install or update Godot without unpacking ZIP archives by hand. Versions track the latest stable releases from [godotengine.org](https://godotengine.org/).

## Prerequisites

- **Windows 64-bit** — required to run the generated installers
- **Inno Setup 6** — required only if you compile installers locally; [download](https://jrsoftware.org/isinfo.php)
- **GitHub Actions** — used by default for CI builds (no local Inno Setup needed)

## Installation

### Download a release

1. Open the [Releases](https://github.com/carlosrabelo/godot-installer/releases) page
2. Pick the release for the Godot line you need (`godot3-<version>` or `godot4-<version>`)
3. Download `Godot_v<version>-install_win64.exe` and run the wizard

### Build from source

```bash
git clone https://github.com/carlosrabelo/godot-installer.git
cd godot-installer
```

On Windows, download the matching Godot `win64` ZIP from the [Godot releases](https://github.com/godotengine/godot/releases), extract the `.exe` next to the corresponding `Godot.iss`, then compile with Inno Setup:

```bash
# After placing Godot_v<version>-stable_win64.exe in godot3/ or godot4/
ISCC.exe godot3\Godot.iss
ISCC.exe godot4\Godot.iss
```

Installers are written to `godot3/Output/` and `godot4/Output/`.

## Usage

### Install Godot on a lab machine

```powershell
# Run the downloaded installer (example for Godot 4)
.\Godot_v4.7.1-install_win64.exe
```

Accept the defaults (or choose a custom directory), optionally create a desktop shortcut, and finish the wizard. Godot starts from the Start Menu entry afterward.

### Uninstall

Use **Apps & features** (or the uninstaller from the install directory). When prompted, choose whether to delete remaining data files under the install folder.

## Project Layout

```
godot3/Godot.iss                 # Inno Setup script for Godot 3
godot4/Godot.iss                 # Inno Setup script for Godot 4
.github/workflows/build.yml      # CI: per-line build and release
```

## Development

Godot versions live in a single define per script:

```
#define MyAppVersion "4.7.1"   # godot4/Godot.iss
#define MyAppVersion "3.6.2"   # godot3/Godot.iss
```

Workflow:

1. Update `MyAppVersion` in the line you want to bump (`godot3/Godot.iss` or `godot4/Godot.iss`)
2. Push to `master` — CI rebuilds **only that line**, then publishes a release tagged `godot3-<version>` or `godot4-<version>` (skips if the tag already exists)
3. For a manual rebuild, run **Build Installers** via `workflow_dispatch`, choose `godot3` / `godot4` / `both`, and enable **Publish a GitHub Release** only when you want a release

Changing `.github/workflows/build.yml` rebuilds both lines but does not create releases. Artifacts are uploaded per line (`godot3-installer`, `godot4-installer`).

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/description`
3. Commit with Conventional Commits: `git commit -m "feat: bump Godot 4 to 4.7.1"`
4. Push and open a pull request

## License

This repository does not currently include a license file. Godot Engine binaries remain under their [upstream license](https://godotengine.org/license/).
