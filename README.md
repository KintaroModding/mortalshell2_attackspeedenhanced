# Attack Speed Enhanced

The player attacks about 15% faster: a base attack speed bonus of 0.15 instead of 0.

A mod for Mortal Shell II (Unreal Engine 5.6), made with game version `1.12.0`. This README is
for developers. What the mod does, how to install it and its compatibility are in [openmods.md](openmods.md), the mod
page on [openmods](https://openmods.nexus).

## How it works

The game keeps the player's start values in the data table
`/Game/Sparta/Core/Characters/Player/Common/DT_PlayerAttributes` (row struct `AttributeMetaData`, an engine struct):
one row per gameplay attribute, named `<AttributeSet>.<Attribute>`, each with `BaseValue`, `MinValue` and `MaxValue`.
The player's character data `CD_Player` uses it, and so do the decoys (`CD_Decoy`, `CD_Decoy_Bait`, `CD_Decoy_Tiel`);
no enemy uses it. The mod changes one `BaseValue`:

| Row | Game | Mod |
|---|---|---|
| `SpartaCombatSet.AttackSpeedBonus` | 0 | 0.15 |

All other rows are the game's values of the version above.

The game's attack code (C++) reads `AttackSpeedBonus`, so its exact formula is not visible in the assets. The only
asset that changes it is the Warp status effect (`GE_WarpStack`: +0.01 per stack, up to 30 stacks, each with a
duration), which the tarstone "Thief's Stone" (`ID_Melee_GrantWarpOnHit`) grants on hits; the game describes Warp as
"Slightly increases attack speed". So 0.15 works like 15 Warp stacks that never run out (probably 15 % faster
attacks), and Warp still adds to it.

"More Defense and Heal" (`mortalshell2_moredefandheal`) replaces the same table, so only one of the two works. "More
Defense, Heal and Attack Speed" holds the changes of both in one table.

The mod replaces whole assets. A mod that changes the same assets conflicts with this one, and a game update that
changes them needs a new build of the mod.

## Build

Requirements:

- Windows PowerShell 5.1 (part of Windows).
- [UAssetGUI](https://github.com/atenfyr/UAssetGUI/releases) v1.1.0 and [retoc](https://github.com/trumank/retoc/releases)
  v0.1.5. `build.ps1` looks for `_tools\uassetgui\UAssetGUI.exe` and `_tools\retoc\retoc.exe` in the parent folders
  of the project; set `UE5_TOOLS` to a folder that holds both to use another place.
- The game's mapping file, `MortalShell2-1.12.0.usmap`, in `%LOCALAPPDATA%\UAssetGUI\Mappings\`.
- For the check of the build (optional): Python 3, the game installed on the same drive as the project, and, when
  the game's files are encrypted, its AES key in `_tools\gamedata\mortalshell2\game.json` (`{"aesKey": "0x..."}`).

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1             # dist\mortalshell2_attackspeedenhanced_P.pak/.utoc/.ucas
powershell -ExecutionPolicy Bypass -File .\build.ps1 -Deploy     # build, then copy the files to Paks\~mods
powershell -ExecutionPolicy Bypass -File .\deploy.ps1 -Wait      # copy the files when the game is closed
```

The build converts every asset source in `src` (UAssetGUI JSON) into a legacy `.uasset`/`.uexp` (UAssetGUI), packs
them into an IoStore container (retoc), and then checks the result: it converts the container back and compares
each asset with its source (`scripts\check_build.py`).

`deploy.ps1` copies the three files to `<game folder>\MortalShell2\Content\Paks\~mods`. The game folder is
`$env:UE5_GAMES_DIR\Mortal Shell II`; `-GameDir` or `-ModsDir` override it. The running game locks the files, so the
script stops (exit code 2) or, with `-Wait`, waits until the game is closed.

Versioning: every change raises the second number in `mod.json` (1.0 -> 1.1): `python scripts\bump_version.py`.

## Project layout

```
src/                               Mod source: the changed assets as UAssetGUI JSON, in the game's folder layout
dist/                              Build output: the .pak/.utoc/.ucas for Paks\~mods (git-ignored)
docs/                              Cover and screenshots for openmods.md
scripts/check_build.py             Compares the built assets with their sources
scripts/bump_version.py            Raises the version in mod.json
build.ps1                          Builds dist\ from src\ and checks the result
deploy.ps1                         Copies the build to the game's Paks\~mods folder
mod.json                           Name, version, game, engine and file names
```

The first commit with an asset holds the original asset, so `git diff` against it shows exactly what the mod
changes.

## License

MIT, see [LICENSE](LICENSE).
