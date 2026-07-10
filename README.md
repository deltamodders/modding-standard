_Standard Revision 4_<br />

# Deltamod Modding Standard
This file includes information on how a Deltamod-compatible modpack should be structured.

This type of pack is recommended for usage as it is compatible with the most popular mod managers: **Deltamod** (by the Deltamod & GM3P development team) and partial support on **G3M** (formerly Deltahub, by Y114).

Additionally, with the newest additions to Deltamod, this format may be the best one for creating GameMaker modpacks, as Deltamod is implementing more GameMaker games alongside DELTARUNE, the main game it was made for.

## Basic rules of the format.
There should be 3 files (1 of which optional) dedicated to mod metadata and patching data.
- `meta.toml` is a file dedicated to storing the mod's name, authors, version and target game. It is mainly used in UI and compatibility checks.
- `modding.xml` consists of <patch> tags, which have three fields: `patch`, `to` and `type`. It is used to define what Deltamod should do to apply your mod.
- `icon.png` is an optional icon the modder can include in their modpack. It must be a 1:1 image (256x256 reccomended but can be any size). It isn't included in this example folder. It is recommended to add one, as it is used in the Deltamod UI and helps the user distinguish mods.

## `meta.json`
> [!NOTE]
> You can use the [MiscTools](https://gamebanana.com/tools/21003) to generate this file. Make sure to select the TOML export.

```toml
deltaruneTargetVersion = "" # Remove if a non-DELTARUNE mod

[metadata]
name = "My Mod"
version = "1.0.0"
description = "My Description"
author = ["Mod Author 1", "Mod Author 2"]
url = "https://example.com"
game = "toby.deltarune" # See below for valid game codes
packageID = "test.pid.here" # See below for package ID styling guide
mergeSupport = true

[color]
r = 0
g = 0
b = 0

[[neededFiles]]
file = "./chapter1_windows/data.win"
checksum = "MY_CHECKSUM"
```
This is an example on how a `meta.toml` should be structured. Deltamod checks the file is valid and has the most important parts of it before loading the mod.<br /><br />
Deltamod will still import mods with a `meta.json`, and will convert them to TOML on load.

### `[[metadata]] mergeSupport`
If set to false, this variable will disallow the user from merging your mod with others.<br />
**We highly recommend turning this on only if absolutely neeeded!** Users like to use multiple mods.

### `[[metadata]] game`
Supported game IDs (which can be added in the `game` field) are:
- `toby.deltarune` (DELTARUNE)
- `toby.deltarune.demo` (DELTARUNE Ch1&2)
- `toby.deltarune.demolts` (DELTARUNE Ch1&2 - LTS version)
- `fans.utyellow` (UNDERTALE Yellow)
- `toby.undertale` (UNDERTALE)
- `other.pizzatower` (Pizza Tower)
  
More games may be added as time goes on. Deltamod will update mods with the `demoMod` field automatically for legacy support.<br />

### `[[metadata]] packageID`
You can format your packageID like so: 
```
websitename.modname.authorname
```
Remember that dots cannot be entered in any of the fields as they are a reserved character. Only use them for separating the blocks.<br /><br />
If you don't want to specify one or more of the package parts (like your username, website name or mod name) just replace the values with `und`. <b>If all three parts are `und`, making a package ID `und.und.und` Deltamod will consider the ID invalid.</b><br /><br />
If an ID is invalid or missing, Deltamod will generate one based on the already present information.

### `deltaruneTargetVersion`
If your mod is not for DELTARUNE, leave the space empty. This is used by G3M to distinguish mods between DELTARUNE versions.<br /><br />
_A future version of Deltamod will also implement this to distinguish Ch5 mods from Ch3+4 mods._

### `neededFiles`
In the `neededFiles` array, you can make sure your game files are the same ones as the ones on your user's computer. Every element in the array is one filename + hash combo.<br /><br />
For example, you can specify the **SHA256 checksum** to look for in the Chapter 4 `data.win`.
You can use checksumming to check, for example, if your user has the same game version as you.<br /><br />
<i>To get a file's SHA256 hash, you can drop it [here](https://emn178.github.io/online-tools/sha256_checksum.html).</i>

## `modding.xml`
> [!NOTE]
> You can use the [MiscTools](https://gamebanana.com/tools/21003) to generate this file.

> [!NOTE]
> `override` and `copy` behave the same. The name difference is only to clear out confusion. `copy` will also override files, and `override` will also copy files.

```xml
<patch type="xdelta" patch="./example.xdelta" to="./chapter3_windows/data.win" />
<patch type="override" patch="./example.win" to="./chapter4_windows/data.win" />
```

This is an example on how a `modding.xml` should be correctly structured. There are currently 2 types of patch: **xdelta** _(which inputs the file through the patcher in order to patch the requested file)_ and **override** (which simply replaces the file or copies it)<br /><br />
Every patch tag has three necessary fields: `type`, `patch`, and `to`. 
- type is either `xdelta`, `override`, `copy` and `g3mpatch` (last 2 only for Deltamod 2.0)
- patch is the relative path to your patch (relative to modpack)
- to is the relative path to the destination file (relative to game folder)
If there are any missing/invalid fields, Deltamod will invalidate that specific patch.

**xdelta** type supports the following file extensions: '.xdelta', '.vcdiff', '.csx', and '.win' <br /> <br />
**override** type supports all files except the following: '.xdelta', '.vcdiff', and '.csx' <br /> <br />
**copy** type supports all files except the following: '.xdelta', '.vcdiff', and '.csx'. It is only an alternative name for override. <br /> <br />
**g3mpatch** type only supports '.g3mpatch' files.<br /> <br />

## The `screenshots` folder
Starting in a future version of Deltamod (TBD), you will be able to put a maximum of 10 screenshots in your modpack's screenshots folder. The format must be PNG, and preferrably of a 4:3 aspect ratio (640x480 ratio or higher). The image might get squished if not of the size here indicated.

## Preparing your archive

Deltamod supports .ZIP (Preferred format for compatibility with most websites), .7Z, .TAR.GZ and .LZMA archives. They must be packaged like so:
```
.
└── (root of the archive)/
    ├── meta.toml
    ├── modding.xml
    ├── icon.png
    └── (any needed patch file)
```

> [!CAUTION]
> One of the most common errors modmakers make when creating modpacks are creating a ZIP with a subfolder in them, and then putting their mod's contents in it. **Your modpack will not work if you make this mistake!** Make sure every file is at the root of the ZIP.

# You're done!
Your mod is now ready to be used in Deltamod.
