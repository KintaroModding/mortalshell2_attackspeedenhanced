# Attack Speed Enhanced

You attack about 15% faster: a base attack speed bonus of 0.15 instead of 0, like 15 stacks of Warp that never run out.

## What it does

You attack faster, from the start of the game.

| | Game | With the mod |
|---|---|---|
| Base attack speed bonus | 0 | 0.15 (probably 15% faster attacks) |

- The game's own attack speed bonus is Warp: the tarstone "Thief's Stone" gives you Warp stacks when you strike foes,
  0.01 attack speed per stack, up to 30 stacks for a short time. The mod's 0.15 works like 15 Warp stacks that never
  run out, and Warp still adds to it.
- It changes only the player's start values (and those of the player's decoys). Enemies stay as in the game.

## Screenshots

![Attack Speed Enhanced](docs/cover.png)

## How to install

1. Download the zip of the latest release on GitHub (`mortalshell2_attackspeedenhanced_<version>.zip`).
2. Open the game folder `MortalShell2\Content\Paks\`. If it has no folder `~mods`, create it.
3. Unpack the three files of the zip (`mortalshell2_attackspeedenhanced_P.pak`, `.utoc` and `.ucas`) into `MortalShell2\Content\Paks\~mods\`.
4. Start the game.

To update the mod, replace the three files with the ones of the new release. To remove it, delete the three files.

## How to use

Nothing to do: the mod works as soon as you load a game.

## Compatibility

- Game version: 1.12.0 (`1.12.0`).
- The mod replaces one game file: the player's attribute table `DT_PlayerAttributes`. A mod that replaces the same
  file conflicts with this one; only one of them works.
- "More Defense and Heal" replaces the same table. To get both, install "More Defense, Heal and Attack Speed" instead
  of the two: it holds the changes of both in one table.
- A game update that changes this table needs a new build of the mod. Until then, the mod puts the older version of
  the table back; remove the mod after a game update until a new version is out.

## Known issues

None yet.

## How to build

Anyone can build this mod from its source code and check that the download matches it.

1. Install the tools: Windows PowerShell 5.1 (part of Windows), [UAssetGUI](https://github.com/atenfyr/UAssetGUI/releases)
   v1.1.0 and [retoc](https://github.com/trumank/retoc/releases) v0.1.5 in a folder `_tools\uassetgui\` and
   `_tools\retoc\` above the repository (or set `UE5_TOOLS`), and the game's mapping file `MortalShell2-1.12.0.usmap` in
   `%LOCALAPPDATA%\UAssetGUI\Mappings\`. `README.md` has the details.
2. Build it: `powershell -ExecutionPolicy Bypass -File .\build.ps1`
3. Find the files: `dist\mortalshell2_attackspeedenhanced_P.pak`, `.utoc` and `.ucas`.

## Credits

Made by Kintaro. MIT license: you may use, change and share this mod. Mortal Shell II and its files belong to
their makers; the mod changes only what is listed above.
