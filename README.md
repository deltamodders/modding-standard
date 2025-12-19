_Standard Revision X_<br /><br />
_Compatibility table:_
|             | 1.0 | 1.1 | 1.1.1 | 1.2 | 1.3 | 1.3.1 | 1.4 (Future) |
|-------------|-----|-----|-------|-----|-----|--------|-----|
| Compatible? |  ❌  |  ❌ |   ❌   |  ❌  |  ❌  |  ⚠️ Take precautions to make your modpack work  | ✅ Fully supported |

_Make sure to read the entirety of this document for more info about forward compatibility. [You can read more here. (Other games paragraph)](#other-games)_

# Deltamod Modding Standard
This file includes information on how a Deltamod-compatible modpack should be structured.

This type of pack is recommended for usage as it is compatible with the most popular mod managers: **Deltamod** (by Deltamodders) and **Deltahub** (by Y114), with partial support.

Additionally, with the newest additions to Deltamod, this format may be the best one for creating GameMaker modpacks, as Deltamod is slowly implementing more GameMaker games alongside DELTARUNE, the main game it was made for.

## Basic rules of the format.
There should be 3 files (1 of which optional) dedicated to mod metadata and patching data.
- `meta.json` is a file dedicated to storing the mod's name, authors, version and mod type (full game or demo). It is mainly used in UI and compatibility checks.
- `modding.xml` consists of <patch> tags, which have three fields: `patch`, `to` and `type`. It is used to actually patch the game and install your mod.
- `icon.png` is an optional icon the modder can include in their modpack. It must be a 1:1 image (256x256 reccomended but can be any size). It isn't included in this example folder.

You can use the [MiscTools](https://gamebanana.com/tools/21003) to create the `meta.json` and `modding.xml` file.

## `meta.json`

```json
{
    "metadata": {
        "name": "example",
        "version": "1.0.0",
        "color": {
            "r": 255,
            "g": 255,
            "b": 255
        },
        "description": "Lorem ipsum",
        "author": ["Mod Developer 1", "Mod Developer 2"],
        "demoMod": true,
        "packageID": "website.mod.author",
        "game": "toby.deltarune"
        "url": "https://example.com",
        "tags": ["other", "customization"]
    },
    "deltaruneTargetVersion": "1.04",
    "neededFiles": [
        {
            "file": "data.win",
            "checksum": "YOUR CHECKSUM HERE"
        }
    ]
}
```
This is an example on how a `meta.json` should be structured. Deltamod checks the file is valid before loading the mod. 

## Other games
With Deltamod 1.4, **Undertale and Undertale Yellow** will receive support in Deltamod. This means that the `demoMod` field is going to be outdated when 1.4 releases.<br /> <br />
Right now, we suggest you use both `demoMod` and `game` in your meta.json **if you are making a DELTARUNE mod**, to enable compatibility with previous versions. In fact, **Deltamod 1.3.1 and DELTAHUB's Deltamod interpreter** do not support multiple games and will not be able to interpret the `game` field appropriately. <br /> <br /> Otherwise, if you're just futureproofing your UNDERTALE/UTYELLOW mod, you can also only indicate the `game` field and not the `demoMod` field.

Supported game IDs (which can be added in the `game` field) are:
- `toby.deltarune` (DELTARUNE)
- `toby.deltarune.demo` (DELTARUNE Ch1&2)
- `fans.utyellow` (UNDERTALE Yellow)
- `toby.undertale` (UNDERTALE)
More games may be added as time goes on.<br /><br />
Here's a schematic table to help you decide which fields to add.

| | Making DELTARUNE mods | Making UNDERTALE/UTYELLOW mods |
|-|----------------------|------------------------|
|Use the `game` field | Yes (Use new Standard X) | Yes (Use new Standard X) |
|Use the `demoMod` field | Yes (Ensure forward compatibility) | No (Older versions won't mod UNDERTALE or others) |

MiscTools will release an update alongside 1.4's initial release to add support for other games. For now, modders wishing to futureproof their mods may use this standard.

## DELTAHUB-specific fields
Starting with DELTAHUB <i>"2.1.0 STABLE"</i>, this mod format will be also implemented in [**DELTAHUB**](https://gamebanana.com/tools/20615).<br /><br /> As part of our deal to merge formats, we've added new fields needed for **DELTAHUB** to function. While Deltamod does not require these strictly, it is reccomended to add them, even if you don't want to make your mod compatible with DELTAHUB.<br /><br />
You will need to add the `url`, `tags` and `deltaruneTargetVersion` fields, which are mirrored from the DELTAHUB specific standard. 
- The `url` field is just a link to your mod page.
- The `tags` field is an array of tags that describe your mod. The supported tags are `textedit, customization, gameplay, other`.
- The `deltaruneTargetVersion` is the DELTARUNE version needed by your mod. Deltamod does not perform checks on target version, but instead uses `neededFiles`.
  **This field is only needed when making a DELTARUNE mod.**

## `packageID`
Starting with Deltamod <i>1.2</i>, all Deltamod mods should have an unique `packageID`.<br /><br />
_The system works similarly to the Android system, where for example Minecraft is assigned the ID `com.mojang.minecraftpe`.<br /><br />_
You can format your packageID like so: 
```
website.modname.authorname
```
Remember that dots cannot be entered in any of the fields as they are a reserved character. Only use them for separating the blocks.<br /><br />
If you don't want to specify one or more of the package parts (like your username, website name or mod name) just replace the values with `und`. <b>If all three parts are `und`, making a package ID `und.und.und` Deltamod will consider the ID invalid.</b><br /><br />
If an ID is invalid or missing, Deltamod will generate one based on the already present information.

## `neededFiles`
In the `neededFiles` array, you can make sure your game files are the same ones as the ones on your user's computer.<br /><br />
For example, you can specify the **SHA256 checksum** to look for in the Chapter 4 `data.win`.
You can use checksumming to check, for example, if your user has the same game version as you.<br /><br />
<i>To get a file's SHA256 hash, you can drop it [here](https://emn178.github.io/online-tools/sha256_checksum.html).</i>
## `modding.xml`

```xml
<patch type="xdelta" patch="./example.xdelta" to="./chapter3_windows/data.win" />
<patch type="override" patch="./example.win" to="./chapter4_windows/data.win" />
```

This is an example on how a `modding.xml` should be correctly structured. There are currently 2 types of patch: **xdelta** _(which inputs the file through GM3P in order to patch the requested file)_ and **override** (which simply replaces the file)<br /><br />
Every patch tag has three necessary fields: `patch`, `to`, and `type`. If there are any missing/invalid fields, the program will invalidate that specific patch.

**xdelta** type supports the following file extensions: '.xdelta', '.vcdiff', '.csx', and '.win' <br /> <br />
**override** type supports all files except the following: '.xdelta', '.vcdiff', and '.csx' <br /> <br />

## The `screenshots` folder
Starting with Deltamod **1.4**, you will be able to put a maximum of 10 screenshots in your modpack's screenshots folder. The format must be PNG, and preferrably of a 4:3 aspect ratio (640x480 ratio or higher). The image might get squished if not of the size here indicated.

## Packing an Archive
Deltamod supports .ZIP (Preferred format for compatibility with most websites), .7Z, .TAR.GZ and .LZMA archives. They must be packaged like so:
```
.
└── (root of the archive)/
    ├── meta.json
    ├── modding.xml
    ├── icon.png
    └── (any needed patch file)
```
## License
This standard is licensed under a modified version of the EUPL, _EUPL-1.2-DELTAMOD_. Read it [here](./LICENSE.txt).
**TLDR: The license allows Deltamod makers to remove your right to use the standard, but we pledge to only use it when highly needed.**
