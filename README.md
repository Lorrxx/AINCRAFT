<p align="center">
  <img src="logo.png" alt="AINCRAFT" width="640">
</p>
<p align="center"><strong>An Aincrad-inspired fantasy RPG experience, built in Minecraft.</strong></p>
<p align="center">Minecraft Java 1.21.4 · Fabric · Custom Windows launcher · In development</p>

![Aincrad Minecraft scene used in the AINCRAFT launcher](aincrad.png)

## Enter AINCRAFT

AINCRAFT is an independent fantasy RPG project built on Minecraft Java Edition. Inspired by Aincrad and Sword Art Online, the project brings a modded Minecraft world and a dedicated launcher together into one experience. Its aim is to make joining the project straightforward: prepare the right game files, connect a Minecraft account, and enter the multiplayer world.

The project is actively being developed. The imagery above shows the Minecraft setting used by the launcher; it does not represent a finished or publicly released game.

## One launcher, one game setup

The custom launcher gives AINCRAFT its own installation and settings, separate from a player's existing Minecraft profiles.

- **Install before playing.** Required game files must be present and complete before Play becomes available.
- **Verify and repair.** Bundled content is checked against its manifest; partial downloads can be retried.
- **Use a Minecraft account.** Microsoft sign-in is handled on Microsoft's own website, with a Java profile required before launch.
- **Join directly.** The launcher prepares the Fabric client and passes the AINCRAFT multiplayer connection to Minecraft.
- **Keep player settings.** Installation and updates preserve player settings and game data.
- **Keep the interface simple.** The launcher uses the full AINCRAFT branding, Minecraft pixel typography, and a static Aincrad screenshot.

| Development platform | Current target |
| --- | --- |
| Game | Minecraft Java Edition 1.21.4 |
| Mod loader | Fabric 0.20.0 |
| Java runtime | Java 21, prepared by the launcher |
| Desktop app | Windows x64, Electron |
| Account integration | Microsoft MSAL device-code flow, Xbox and Minecraft Services |
| Distribution | Local development builds; no public release |

## Microsoft and Minecraft application review

AINCRAFT uses its **own** Microsoft application registration. Players will not need to register an Azure application or supply their own client ID.

**Application name:** AINCRAFT  
**Public application/client ID:** `9cdecb3f-dda2-46ab-81ce-8104ed0762c8`  
**Account audience:** Personal Microsoft accounts  
**Client type:** Public desktop client; no client secret

Minecraft Services API access is needed to exchange the player's authorized Xbox identity for a Minecraft session and retrieve their Java profile. This supports account validation and launching the player's owned game. The launcher does not offer offline-account bypasses or reuse another launcher's application ID.

### How sign-in works

1. The player chooses **Sign in** in the launcher.
2. MSAL requests a device code using AINCRAFT's application ID and the `XboxLive.signin` scope.
3. The player opens Microsoft's sign-in page, enters the code, and reviews Microsoft's consent screen.
4. The launcher completes the Xbox Live and XSTS exchange, then requests a Minecraft Services session and Java profile.
5. Only a valid Java profile enables an authenticated launch. The Minecraft session stays in the desktop main process until it is supplied to the game.

`Microsoft sign-in → Xbox Live → XSTS → Minecraft Services → Java profile → AINCRAFT launch`

| Service | Purpose |
| --- | --- |
| Microsoft identity platform | Player authentication and device-code consent |
| Xbox Live / XSTS | Authorize the Xbox identity for Minecraft Services |
| Minecraft Services | Obtain a Minecraft session and retrieve the Java profile |
| AINCRAFT multiplayer server | Receive the normal Minecraft connection after launch |

### Account data and privacy

Microsoft passwords and verification codes are entered only on Microsoft's website. AINCRAFT does not collect them in its own UI. The launcher does not request mailbox, calendar, contact, or cloud-file access.

Authentication runs in the Electron main process; the renderer receives the player's public profile rather than tokens. The local MSAL session cache is encrypted through Electron safeStorage using the current Windows account. Disconnect removes that local cache; provider-side consent can be managed separately through Microsoft.

For portable builds, settings and the encrypted cache live in the launcher's local data directory. They are not included in this repository or in the public documentation.

## Development and release status

| Area | Status |
| --- | --- |
| Launcher installation, file checks, settings and interface | Implemented and tested locally |
| AINCRAFT Microsoft app registration | Created; personal accounts and public-client flow enabled |
| Microsoft device-code issuance | Verified against the live Microsoft endpoint |
| Minecraft Services application review | Not yet completed |
| Owning-account sign-in and authenticated gameplay | End-to-end verification still required |
| Public downloads and source distribution | Not published |

This repository is the **public project overview and application-review reference**. It contains this README and the two branding images used above. Launcher source code, game files, executables, authentication data, and releases remain private. There is no announced public release date.

Project questions can be raised through [GitHub Issues](https://github.com/Lorrxx/AINCRAFT/issues).

---

AINCRAFT is an independent fan project. It is not an official Minecraft or Sword Art Online product and is not approved by or associated with Mojang, Microsoft, or the Sword Art Online rights holders. Minecraft and other referenced names belong to their respective owners.
