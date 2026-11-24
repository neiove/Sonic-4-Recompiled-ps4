# Sonic 4 Episode I Recompiled — PS4

An unofficial PlayStation 4 homebrew port of **Sonic the Hedgehog 4: Episode I**, built from the Xbox 360 version using static recompilation.

The project brings the game to PS4 with controller support, audio, save loading, a mod menu, PlayStation button replacements, and an experimental playable Tails mod.

**Status:** Work in progress · **Development snapshot:** v0.53 · **Performance target:** 60 FPS

[Join the Discord server](https://discord.gg/mFAfA3tq4) for project discussion, help, and feedback.

> [!IMPORTANT]
> **You must provide your own `data.cpk` and `default.xex` from your own legally obtained Xbox 360 copy of Sonic the Hedgehog 4: Episode I.**
>
> These files are not included. The port cannot run without them. Do not upload game dumps or original game files to this repository or its issues.

## Features

- Recompiled Xbox 360 game code with a PS4 runtime.
- Rendering on the PS4 GPU, with translated Xbox 360 shaders.
- PS4 controller input, audio output, and save loading.
- An in-game menu opened with **L3 + R3**.
- A Mods tab and a built-in **PlayStation Buttons** mod, enabled by default.
- An experimental **Tails** character mod using assets from your own Episode II copy.
- Logging and frame-capture tools for troubleshooting and development.

## Requirements

- A homebrew-capable PS4 or PS4 Pro that can install and run the project's PKG.
- The project's PS4 PKG.
- Your own **Episode I Xbox 360** game files:
  - `default.xex`
  - `data.cpk`
- A way to transfer files to the PS4, such as FTP.
- **Optional, for Tails:** your own `data.cpk` from the **Xbox 360 version of Episode II**.

Episode I and Episode II use the same archive filename. Keep them in their separate folders.

## Installation

1. Install the project's PKG on your PS4:

   ```text
   IV0000-SONC00004_00-SONIC4EP1RECOMP0.pkg
   ```

2. Launch it once to create its data folders, then close it. You can also create the folders manually through FTP.
3. Copy your Episode I files to the following locations:

   | File | Destination on PS4 |
   | --- | --- |
   | Episode I `default.xex` | `/data/Sonic4Recomp/game/default.xex` |
   | Episode I `data.cpk` | `/data/Sonic4Recomp/game/data.cpk` |

4. Launch the game again.

Keep the filenames exactly as shown. Xbox Live extras such as achievement images and `ArcadeInfo.xml` are not required by the documented setup.

## Controls and mods

Press **L3 + R3** to open the in-game menu.

The built-in **PlayStation Buttons** mod replaces several Xbox prompts with PlayStation equivalents, including Cross, Circle, Triangle, and L1/R1. It is enabled by default and appears in the Mods tab.

Some Xbox prompts remain, and the controller illustration still uses an Xbox-shaped controller.

### Playable Tails

Tails is an experimental addition with flight, reduced speed, character-specific effects, a Tails HUD, and a Super Tails appearance.

To set it up:

1. Copy your own **Episode II** `data.cpk` to:

   ```text
   /data/Sonic4Recomp/ep2/data.cpk
   ```

2. Launch the game. If the `ep2` folder is missing, create it manually.
3. Enter the character unlock sequence using the **D-pad**:

   **Up → Up → Down → Down → Left → Right → Left → Right**

4. Select **Tails** in the character selector, then restart the game.

The game generates the needed Tails files on startup. The first launch after an update that rebuilds these files may take a few extra seconds.

**Do not replace Episode I's archive with Episode II's archive.** The main game still needs both Episode I files in the `game` folder.

In v0.53, holding **Cross (X)** is intended to keep Tails flying upward. That build also includes changes intended to block homing attacks and air dashes, including after springs. These changes still need console validation.

## Current status and known issues

The development history reports gameplay, audio, save loading, and rendering working in earlier builds. **The latest build described here, v0.53, was built but had not yet been tested on PS4.**

Known issues and incomplete work include:

- **Performance:** the target is 60 FPS, but a fully locked 60 FPS is not guaranteed.
- **Rendering:** water appearance remains an open issue. Report missing graphics or effects with the affected stage and screenshots.
- **Audio:** music was reported to go silent during a longer session; the cause was not confirmed.
- **Button prompts:** some Xbox icons are still present.
- **Tails:** animations and effects are still being refined. Earlier builds could retain Sonic's homing attack or air dash in some situations; v0.53 attempts to fix this.
- **Super Tails:** uses a Tails spin-and-pose transformation. Sonic's original transformation animation has not been adapted, and surrounding Flickies are not implemented.
- **Vines and ropes:** the requested animation replacements are not implemented.
- **Character menu:** v0.53 changes the selector to open on the current character; persistence and selection behavior need further testing.
- **Tails visual corruption:** v0.53 compresses generated assets as a proposed fix; the result is unverified.

### Performance notes

Earlier **v0.43 testing on a PS4 Pro with Boost Mode disabled** reportedly had about **98.7% of frames complete within one refresh**, across roughly 4,800 frames.

That is a result from one recorded test, not a guarantee for every stage or console. A PS4 Pro with Boost Mode disabled is not a substitute for testing a base PS4 or PS4 Slim. Later builds also need their own performance checks.

Optimization work has included GPU submission batching, faster index processing, cheaper draw lookups, constant-buffer reuse, and command-stream parsing improvements.

## Troubleshooting and bug reports

### The game cannot find its files

Check that both Episode I files are directly inside:

```text
/data/Sonic4Recomp/game/
```

For Tails, the separate Episode II archive belongs inside:

```text
/data/Sonic4Recomp/ep2/
```

### Black screen, freeze, or crash

Collect the logs from:

```text
/data/Sonic4Recomp/Logs/
```

Useful files include:

- `boot.log`
- `latest.log`
- Any `crash_*.log` files

A renderer fallback was documented during development: create an empty file named `ps4_no_renderer` at:

```text
/data/Sonic4Recomp/ps4_no_renderer
```

Restart the application to use the diagnostic status screen. Remove the file and restart to re-enable rendering.

### What to include in a report

Open a GitHub issue or share your findings in [Discord](https://discord.gg/mFAfA3tq4). Include:

- Build version.
- Console model and whether Boost Mode is enabled.
- Stage, act, and selected character.
- Enabled mods.
- Steps to reproduce the problem.
- Relevant logs and a screenshot or short video.

If requested, provide diagnostic captures from `/data/Sonic4Recomp/capture/` or recordings from `/data/Sonic4Recomp/ps4_record/`.

**Do not attach `data.cpk`, `default.xex`, or complete game dumps to bug reports.**

## Development

The documented PS4 build entry point is:

```powershell
powershell -File tools\build-ps4.ps1 -Version 0.53
```

Building requires the project's PS4 toolchain and dependencies to be configured. This command alone does not install them. The shader workflow also uses PC-side tools and a WSL/RADV packing step.

See the build scripts for tool paths and **`HANDOFF.md`**, when included in the checkout, for runtime details, debugging notes, and the development history.

## Credits

- **SEGA / Sonic Team** — original game and characters.
- **UnleashedRecomp / XenosRecomp** — work used as a foundation for the recompilation and shader tooling.
- **UnleashedRecompPS4 / plume PS4 backend** — graphics backend work adapted for this port.
- **OpenOrbis** — PS4 homebrew development tooling.
- Community testers and contributors.

Retain the applicable upstream license notices when redistributing code or dependencies.

This is an unofficial fan project. It is not affiliated with or endorsed by SEGA, Microsoft, or Sony. Sonic the Hedgehog and related assets belong to their respective owners.

