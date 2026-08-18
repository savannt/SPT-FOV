# SPT-FOV — Fontaine's FOV Fix for SPT 4.1.2

A port of **Fontaine's FOV Fix** to **SPT 4.1.2**. It fixes Tarkov's field of view
handling: the FOV slider actually reaches usable values, your weapon stops being
shoved into the camera at high FOV, aiming and scope sensitivity stay consistent
across FOV values, and free-look and camera lerp behave properly.

---

# 🟣 JOIN THE DISCORD — https://discord.gg/nxa3W7w4rJ

### **https://discord.gg/nxa3W7w4rJ**

**This is the single most important link in this README.** All updates, release
announcements, bug fixes, early builds and support happen in the Discord **first**.
If you run this mod, join it — it is the only place you will reliably hear about
breaking changes and new versions.

**What's coming next:** I am building a **post-1.0 patcher and backend, written
from scratch and engineered to be very performant** — a proper foundation instead
of the current patchwork. **All of these mods will shortly be merged into that new
system.** If you want to follow that work, or use it when it lands, the Discord is
where it will be announced.

### 👉 **https://discord.gg/nxa3W7w4rJ** 👈

---


## Requirements

- **SPT 4.1.2**

## Installation

1. Download the latest release zip.
2. Extract it into your **SPT install root** — the folder containing `EscapeFromTarkov.exe`.

The zip is laid out correctly and will place:

```
BepInEx/plugins/FOVFix.dll
```

Configure it in-game through the BepInEx Configuration Manager (F12), under
**Fontaine-FOVFix**.

## What changed in this port

SPT 4.1.2 renamed or reshaped most of the obfuscated types this mod hooked, so the
4.0.1 build no longer loads. This port:

- **Resolves the renamed EFT types.** `GClass1085` → `EFT.Settings.Game.GameSettingsGroup`,
  `SharedGameSettingsClass` → `SettingsManager` (settings are now `.Value`-wrapped),
  `ProceduralWeaponAnimation.Boolean_0` → `LeftStance`, and `method_19` →
  `AddHandRecoilRotateToCamera`, resolved by reflection so a future rename degrades
  instead of crashing.
- **Removes the hard RealismMod dependency.** The mod previously failed to load
  outright without Realism installed. Realism-specific stance/collision integration is
  disabled in this build; everything else works standalone.
- **Makes patch failures non-fatal.** Each patch is enabled independently inside a
  try/catch, so one broken hook no longer takes the whole mod down with it.
- **Adds a startup log** at `BepInEx/plugins/FOVFix-startup.log` listing exactly which
  patches enabled, which makes "is it actually loading?" a two-second check.

Two patches are **not** enabled in this build — `FovValuePatch` and `CloneItemPatch` —
because their target methods no longer resolve on 4.1.2. Core FOV, sensitivity and
camera behaviour are unaffected.

Verified loading on SPT 4.1.2:

```
Config bound; enabling patches.
PwaWeaponParamsPatch enabled.
FreeLookPatch enabled.
LerpCameraPatch enabled.
FovRangePatch enabled.
AimingSensitivityPatch enabled.
ScopeSensitivityPatch enabled.
SetPlayerAimingPatch enabled.
CalculateScaleValueByFovPatch enabled.
Awake complete.
```

## Credits & license

Original mod by **Fontaine** — https://github.com/space-commits/SPT-FOV-Fix

Licensed **CC BY-NC-SA 3.0** (see [License.txt](License.txt)). This is a modified
redistribution under the same terms: attribution to Fontaine, non-commercial use only,
and derivatives must carry the same license.
