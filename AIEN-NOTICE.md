# AIEN notice

This repository is the AIEN (aien-dev) copy of the Raspberry Pi 5 UEFI firmware
originally published by worproject. AIEN keeps its own copies of every piece the
firmware is built from, so builds keep working even if an original repository
moves, changes or disappears.

## The firmware chain

The firmware image is built from four source repositories, each included here as
a submodule (a pinned link to an exact version of another repository):

| Submodule | AIEN copy | Branch | Original |
|---|---|---|---|
| arm-trusted-firmware | aien-dev/arm-trusted-firmware | rpi5 | worproject/arm-trusted-firmware |
| edk2 | aien-dev/edk2 | sdmmc-dev | worproject/edk2 |
| edk2-platforms | aien-dev/edk2-platforms | rpi5-dev | worproject/edk2-platforms |
| edk2-non-osi | aien-dev/edk2-non-osi | master | tianocore/edk2-non-osi |

The submodule links point at the AIEN copies. The pinned versions are the same
ones the original project uses, so the firmware source is unchanged.

## Keeping in sync

The AIEN copies of the four source repositories carry no AIEN-specific files on
their branches, so they can be updated from the originals cleanly.
