# Raspberry Pi 5 UEFI (AIEN preservation fork of worproject/rpi5-uefi)

> TF-A + EDK2 UEFI firmware for the Raspberry Pi 5, kept by AIEN so the firmware can still be built if the original moves or disappears. Upstream `worproject/rpi5-uefi` is archived and no longer maintained. AIEN's only changes are on `master`: the four submodules point at AIEN copies (same branches, same pinned commits), plus `AIEN-NOTICE.md` and a fork note at the top of `README.md`.
Shared rules: read https://www.aienos.com/agents/RULES.md first. This file adds what is specific to this repository; where the two differ, this file wins here.

## Scope

- Applies to the whole repository. This is a fork, not an AIEN project; the firmware source is upstream's, unchanged.
- AIEN's own commits are exactly two, both on `master`:
  - `1f522a5` Point submodules at aien-dev forks and add AIEN notice (#1): edits `.gitmodules`, adds `AIEN-NOTICE.md`.
  - `30e4872` docs: README refresh for new readers (#2): adds the fork note to `README.md`, aligns `AIEN-NOTICE.md`.
  - Every older commit is upstream's history.
- Changes accepted here: AIEN documentation (`AIEN-NOTICE.md`, the fork note at the top of `README.md`, this file), and `.gitmodules` URLs that keep the chain on AIEN copies.
- Must not change: firmware source, `build.sh`, `config.txt`, `.github/workflows/`, the upstream part of `README.md` (everything below the fork note), and the pinned submodule commits. A submodule bump is only acceptable as a sync to what upstream pins, never an AIEN code change.
- The firmware chain (see `AIEN-NOTICE.md`):

| Submodule | AIEN copy | Branch | Pinned commit |
|---|---|---|---|
| `arm-trusted-firmware` | aien-dev/arm-trusted-firmware | `rpi5` | `682607f` |
| `edk2` | aien-dev/edk2 | `sdmmc-dev` | `eca8aae` |
| `edk2-platforms` | aien-dev/edk2-platforms | `rpi5-dev` | `8e1779b` |
| `edk2-non-osi` | aien-dev/edk2-non-osi | `master` | `1f4d784` |

## Installation

```bash
git clone --recurse-submodules https://github.com/aien-dev/rpi5-uefi.git
cd rpi5-uefi
```

Build dependencies on Ubuntu/Debian, from `README.md` (needs sudo; the list is not complete, see the README):

```bash
sudo apt install git gcc g++ build-essential gcc-aarch64-linux-gnu iasl python3-pyelftools uuid-dev
```

CI (`.github/workflows/build.yml`) installs `acpica-tools binutils-aarch64-linux-gnu build-essential device-tree-compiler gettext git gcc-aarch64-linux-gnu libc6-dev-arm64-cross python3 python3-pyelftools` on `ubuntu-latest`.

## Configuration

- `.gitmodules`: submodule URLs and branches. All four point at `https://github.com/aien-dev/...`.
- `config.txt`: Raspberry Pi boot configuration shipped next to the image (`armstub=RPI_EFI.fd`, device tree address, framebuffer, USB current, `force_turbo=1`). Upstream file; do not edit.
- `build.sh` options: `--model 4|5` (default 5), `--debug 0|1` (default 0), `--tfa-flags "FLAGS"`, `--edk2-flags "FLAGS"`, `--help`. On a non-aarch64 host it sets `CROSS_COMPILE=aarch64-linux-gnu-` unless already set.

## Usage

```bash
# Show build options (cheap, no build)
./build.sh --help

# Full TF-A + EDK2 build; writes RPI_EFI.fd at the repository root
./build.sh
./build.sh --debug 1
```

- A full build compiles TF-A, EDK2 BaseTools and the RPi5 platform; it is slow and needs the submodules checked out. Do not run it to validate a documentation change.
- Flashing and booting need a Raspberry Pi 5 (tested upstream only on early BCM2712C1 boards; see the end of support notice in `README.md`). Copy `RPI_EFI.fd`, `config.txt` and `bcm2712-rpi-5-b.dtb` to a FAT32 boot partition. Serial console: 115200 8n1.
- CI: `build.yml` builds Debug and Release for model 5 on pushes and pull requests to `master`, skipping changes that touch only `**.md`. `release.yml` builds and creates a draft GitHub release on any pushed tag. Do not push tags.

## Conventions

- Keep the fork diffable against upstream: AIEN changes stay in `AIEN-NOTICE.md`, the README fork note, this file and `.gitmodules`.
- Submodule URLs must keep pointing at AIEN copies with the same branch names as upstream.
- No em dashes or en dashes in AIEN-authored files.

## Safety and approvals

- Ask before any change outside the accepted list in Scope, before bumping a submodule pin, and before pushing a tag (a tag triggers `release.yml`).
- Never rewrite `master` history or force-push; other repositories and old builds reference these commits.
- Never run firmware builds or flash images as a check for documentation-only changes. Flashing needs physical Raspberry Pi 5 hardware and can leave a board unbootable.
- Never commit build output (`Build/`, `*.fd` are in `.gitignore`).

## Further reading

- `README.md`: AIEN fork note, then the upstream README (support status, configuration menus, supported OSes, build steps, licenses).
- `AIEN-NOTICE.md`: the firmware chain and how the AIEN copies stay in sync.
- `build.sh`: the exact TF-A and EDK2 build invocations.
- https://github.com/aien-dev/edk2-non-osi: one of the four chain sources.
- https://github.com/worproject/rpi5-uefi: the archived original.
