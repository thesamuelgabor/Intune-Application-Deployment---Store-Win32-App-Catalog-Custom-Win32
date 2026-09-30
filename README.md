# Intune Application Deployment — Store, Win32 App Catalog & Custom Win32

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

Added Company Portal itself as a Store app, assigned as Required to `SG-Win11-Apps-Pilot`.

<img width="800" height="450" alt="image" src="docs/img/01-store-app.png" />

*Ref 1: Store app*

#### 2. Deploy an App From the Win32 App Catalog

Selected a common third-party title (e.g. a PDF reader) directly from Intune's built-in Win32 app catalog — Microsoft maintains the packaging and version updates, so no manual `.intunewin` work was needed for this one.

<img width="800" height="450" alt="image" src="docs/img/02-win32-catalog.png" />

*Ref 2: Win32 app catalog*

#### 3. Package a Fully Custom Win32 App

Packaged an internal line-of-business installer from scratch using the Win32 Content Prep Tool, since it isn't available in the catalog.

```powershell
IntuneWinAppUtil.exe -c "C:\Source" -s "install.exe" -o "C:\Output"
```

| Setting | Value |
|---|---|
| Install command | `install.exe /quiet` |
| Uninstall command | `uninstall.exe /quiet` |
| Detection rule | MSI product code / registry key presence |
| Requirement rule | Minimum OS: Windows 11 23H2, x64 only |

<img width="800" height="450" alt="image" src="docs/img/03-custom-win32.png" />

*Ref 3: Custom Win32 packaging*

#### 4. Compare the Three Methods

| Method | Packaging effort | Update ownership | Best for |
|---|---|---|---|
| Store app | None | Microsoft / publisher | Modern, store-distributed apps |
| Win32 app catalog | None — pre-packaged by Microsoft | Microsoft | Common third-party desktop apps |
| Custom Win32 | Manual packaging + detection rules | You | Internal / line-of-business apps |

*Ref 4: Deployment method comparison*

#### 5. Verify on Device

Confirmed all three apps installed automatically on `SG-Win11-Apps-Pilot` devices in Company Portal, with no user interaction required. This same set of three apps is what the [Autopilot Device Preparation](../03-Windows-Autopilot-Device-Preparation) project assigns to its device preparation policy.

<img width="800" height="450" alt="image" src="docs/img/05-company-portal-verified.png" />

*Ref 5: Verified installation*

## About

The three core Intune application delivery methods — Store, Win32 app catalog, and custom Win32 — packaged, deployed, and compared side by side.
