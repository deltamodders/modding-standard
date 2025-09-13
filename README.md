_Valid for Deltamod, version 1.0-1.1.1_

# Deltamod Standard
This file includes information on how a Deltamod modpack should be structured.

## Basic rules of the format.
There should be 3 files (1 of which optional) dedicated to mod metadata and patching data.
- `_deltamodInfo.json` is a file dedicated to storing the mod's name, authors, version and mod type (full game or demo). It is mainly used in UI and compatibility checks.
- `modding.xml` consists of <patch> tags, which have three fields: `patch`, `to` and `type`. It is used to actually patch the game and install your mod.
- `_icon.png` is an optional icon the modder can include in their modpack. It must be a 1:1 image (256x256 reccomended but can be any size). It isn't included in this example folder.

## `_deltamodInfo.json`

```json
{
    "metadata": {
        "name": "example",
        "version": "1.0.0",
        "description": "Lorem ipsum",
        "author": ["Mod Developer 1", "Mod Developer 2"],
        "demoMod": true,
        "packageID": "website.mod.author",
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
This is an example on how a `_deltamodInfo.json` should be structured. Deltamod checks the file is valid before loading the mod. 

# DELTAHUB-specific fields
Starting with DELTAHUB <i>"2.1.0 STABLE"</i>, this mod format will be also implemented in [**DELTAHUB**](https://gamebanana.com/tools/20615).<br /><br /> As part of our deal to merge formats, we've added new fields needed for **DELTAHUB** to function. While Deltamod does not require these strictly, it is reccomended to add them, even if you don't want to make your mod compatible with DELTAHUB.<br /><br />
You will need to add the `url`, `tags` and `deltaruneTargetVersion` fields, which are mirrored from the DELTAHUB specific standard. 
- The `url` field is just a link to your mod page.
- The `tags` field is an array of tags that describe your mod. The supported tags are `translation, text, customization, gameplay, other`. 
- The `deltaruneTargetVersion` is the DELTARUNE version needed by your mod. Deltamod does not perform checks on target version, but instead uses `neededFiles`.

### `packageID`
Starting with Deltamod <i>1.2</i>, all Deltamod mods should have an unique `packageID`.<br /><br />
_The system works similarly to the Android system, where for example Minecraft is assigned the ID `com.mojang.minecraftpe`.<br /><br />_
You can format your packageID like so: 
```
website.modname.authorname
```
Remember that dots cannot be entered in any of the fields as they are a reserved character. Only use them for separating the blocks.<br /><br />
If you don't want to specify one or more of the package parts (like your username, website name or mod name) just replace the values with `und`. <b>If all three parts are `und`, making a package ID `und.und.und` Deltamod will consider the ID invalid.</b><br /><br />
If an ID is invalid or missing, Deltamod will generate one based on the already present information.

### `neededFiles`
In the `neededFiles` array, you can make sure your Deltarune files are the same ones as the ones on your user's computer.<br /><br />
For example, you can specify the **SHA256 checksum** to look for in the Chapter 4 `data.win`. Look at this graph:<br /><br />
<img width="591" height="221" alt="d1" src="https://github.com/user-attachments/assets/e0476db0-7ba3-4150-8bfb-70779db81805" /><br /><br />
You can use checksumming to check, for example, if your user has the same Deltarune version as you.<br /><br />
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

## Packing an Archive
Deltamod supports .ZIP, .7Z, .TAR.GZ and .LZMA archives. They must be packaged like so:
```
.
└── (root of the archive)/
    ├── _deltamodInfo.json
    ├── modding.xml
    ├── _icon.png
    └── (any needed patch file)
```
## License
This standard is licensed under a modified version of the EUPL, _EUPL-1.2-DELTAMOD_. Read it [here](./LICENSE.txt)
