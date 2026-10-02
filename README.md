# Intune Application Deployment - Store, Win32 App Catalog & Custom Win32

## Objective

This project demonstrates the three ways to deliver software through Intune: a Microsoft Store app, a pre-packaged app pulled from the Intune Win32 app catalog, and a fully custom Win32 app packaged from scratch.
The apps built here are assigned to the devices enrolled in Microsoft Entra Joined/Cloud-Only Windows-11 Autopilot, and the same app set is reused in Windows Autopilot Device Preparation.

### Skills Learned

- Store app deployment and assignment
- Using the Intune Win32 app catalog for pre-packaged third-party apps
- Packaging a custom Win32 app with the Win32 Content Prep Tool
- Designing detection rules and requirement rules for custom packages

### Tools Used

- Microsoft Intune / Endpoint Manager admin center
- Win32 Content Prep Tool (`IntuneWinAppUtil.exe`)
- Company Portal

## Steps

#### 1. Deploy a Microsoft Store App

Added Company Portal itself as a Store app, assigned as Required to `DG-Win11-Autopilot-Pilot`.

<img width="1662" height="852" alt="01-store-app" src="https://github.com/user-attachments/assets/4b6dcab4-f9fb-4365-919b-8e7c29033b01" />

*Ref 1: Store app*

#### 2. Deploy an App From the Enterprise App Catalog

Selected a common third-party title directly from Intune's built-in Enterprise App Catalog - Microsoft maintains the packaging and version updates, so no manual `.intunewin` work was needed for this one.

<img width="1625" height="872" alt="02-win32-catalog" src="https://github.com/user-attachments/assets/4a53f8b8-240a-4778-a37b-2c9be746b913" />

*Ref 2: Win32 app catalog*

#### 3. Package a Fully Custom Win32 App

Packaged an internal line-of-business installer from scratch using the [Microsoft Win32 Content Prep Tool](https://github.com/microsoft/microsoft-win32-content-prep-tool) , since it isn't available in the catalog.

| Setting | Value |
|---|---|
| Install command | `vlc-3.0.24-win64.exe /S` |
| Uninstall command | `%ProgramFiles(x86)%\VideoLAN\VLC\uninstall.exe /S` |
| Detection rule | File or folder exists | %ProgramFiles(x86)%\VideoLAN\VLC\vlc.exe |
| Requirement rule | Minimum OS: Windows 11 24H2 |

<img width="873" height="842" alt="03-custom-win32" src="https://github.com/user-attachments/assets/7ab985f3-9882-432b-9ee2-d69bc3b4aa6d" />

*Ref 3: Custom Win32 packaging*

#### 4. Compare the Three Methods

| Method | Packaging effort | Update ownership | Best for |
|---|---|---|---|
| Store app | None | Microsoft / publisher | Modern, store-distributed apps |
| Win32 app catalog | None - pre-packaged by Microsoft | Microsoft | Common third-party desktop apps |
| Custom Win32 | Manual packaging + detection rules | You | Internal / line-of-business apps |

*Ref 4: Deployment method comparison*
