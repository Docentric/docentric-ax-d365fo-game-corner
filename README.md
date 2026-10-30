# Docentric AX Game Corner for D365FO

This project is a showcase model that opens the Docentric AX **Game Corner** in a standalone window of Microsoft Dynamics 365 Finance and Operations (D365FO). The Game Corner is the `DocGameCornerControl` extensible control included in Docentric AX, with four small canvas games: **Jumpy Van**, **Growing Tail**, **Space Shrinker** and **Bouncing Breaker**. The showcase adds that control on a form of its own and adds a **Docentric game corner (POC)** entry to the standard **Settings** menu. In the product itself the control lives inside the **Bulk file upload** dialog, where it offers a break while files upload.

The games' engine is not part of this repository: it stays in the Docentric AX model as the `DocGameCornerGamesJS` resource (`DocGameCornerGames.js`), next to the control's own `DocGameCornerControl` HTML, script and stylesheet resources. This model only adds the control to a form.

[Docentric AX](https://ax.docentric.com) is a complete reporting and output management solution for Microsoft Dynamics 365 Finance and Operations. The solution provides companies and partners with a better way to create reports and automate the distribution and archiving of business documents.

Two editions of the solution are available:

- Docentric AX Free Edition: Solves most common shortcomings in D365FO output management.
- Docentric AX Full Edition: Report design and advanced output management features.

A complete feature list with a comparison to standard D365FO solutions and what’s part of the Free and Full Edition is described here: https://ax.docentric.com/feature-list.

## Download

Each release contains the source archives and the model file for import with ModelUtil.

[Releases](../../releases)


## Articles

You can learn more about the solution here:
- [Work Hard, Play Hard: The Docentric Game Corner in D365FO](https://ax.docentric.com/work-hard-play-hard-the-docentric-game-corner-in-d365fo)

## Prerequisites

[Docentric AX Free Edition](https://ax.docentric.com/download/) v3.6.0 or later is required. The Game Corner control  is included in the Free Edition as well as in the Full Edition, where it also lives inside the Bulk file upload dialog.

## Features

- **Standalone window**: The `DocGameCornerShowcaseWindow_DCGC` form (dialog pattern, caption **Docentric Game Corner**) contains `DocGameCornerControl` sized to the available area, with a short introduction text above it. The form contains no custom X++ logic: the control renders its own state and does not decide who may use it.
- **Initial game**: The control's `initialGame` design-time property (enum `DocGameCornerGame`: `Hopper`, `Tail`, `Shrinker`, `Bouncer`) selects the game shown first; the showcase starts with **Bouncing Breaker** (`Bouncer`).
- **Settings menu entry**: `SysMenuSettings.Extension_DCGC` adds the `DocGameCornerShowcaseWindow_DCGC` display menu item, labeled **Docentric game corner (POC)**, to the Settings (gear) menu. The (POC) suffix keeps it apart from the product's own Game Corner entry on an installation that carries both.
- **Security**: The `DocentricAXGameCornerShowcaseUser_DCGC` privilege grants access to the showcase window. The games themselves are not governed by a privilege: in the product they are switched on per placement and can be switched off for the environment with the **DisableGameCorner** setting.

## Instructions

The X++ metadata in the repository is under the Metadata folder, which contains all the objects needed. We need to make the model visible to D365FO (at **:\AOSService\PackagesLocalDirectory**) by creating [Symbolic links](https://en.wikipedia.org/wiki/NTFS_links) to the model folder on the filesystem.

> Using the described approach you can use multiple Git repositories (e.g., solutions or customizations) on the same D365FO OneBox environment.

To do so, follow the next instructions (before you start, make sure you have Visual Studio closed):

1. **Add the model to D365FO.** Run the following command in PowerShell as an Administrator. It creates a symbolic link in **:\AOSService\PackagesLocalDirectory** for the model folder of this repository (a model that is already there is skipped):

`.\AOS_CreateMetadataSymbolicLinks.ps1`

2. **Remove the model from D365FO when you no longer need it.** Run the following command in PowerShell as an Administrator. It deletes the symbolic link from **:\AOSService\PackagesLocalDirectory** and leaves the repository itself untouched:

`.\AOS_RemoveMetadataSymbolicLinks.ps1`

> NOTE: Delete only symbolic links, not the source folders!

### Using scripts with Unified Development Environment (UDE)
Both scripts detect the D365FO package's local folder, but with UDE environments, you have a custom metadata folder, which is set through the `'Visual Studio > Dynamics 365 Extension > Configure Metadata'` in setting `'Folder for your own custom metadata'`. To create a symbolic link, use `.\AOS_CreateMetadataSymbolicLinks.ps1 -PackagesLocalFolder "C:\CustomAOS\PackagesLocalDirectory"`.

> NOTE: To remove use the same parameter with script `.\AOS_RemoveMetadataSymbolicLinks.ps1 -PackagesLocalFolder "C:\CustomAOS\PackagesLocalDirectory"`.

## Folder structure

The repository starts with the following folder structure:

- `Metadata`: *Folder containing the D365FO model (`DocentricAXGameCorner`, model name `Docentric AX Game Corner`)*
- `Projects`: *Folder containing Visual Studio files (e.g., solutions, projects gluing the D365FO model artifacts).*
- `AOS_CreateMetadataSymbolicLinks.ps1`: *Script to create the symbolic links to the AosService\PackagesLocalDirectory folder.*
- `AOS_RemoveMetadataSymbolicLinks.ps1`: *Script to remove the symbolic links from the AosService\PackagesLocalDirectory folder.*
- `.gitattributes`: *Defines project-based rules for manipulating files and paths attributes when a Git action is performed (e.g., line-endings, file compare, etc.).*
- `.gitignore`: *Defines project-based rules for ignoring files and folders in the Git repository.*

## Model content

| Object | Type | Role |
|---|---|---|
| `DocGameCornerShowcaseWindow_DCGC` | Form | The standalone window containing `DocGameCornerControl` |
| `DocGameCornerShowcaseWindow_DCGC` | Display menu item | Opens the window; label **Docentric game corner (POC)** |
| `SysMenuSettings.Extension_DCGC` | Menu extension | Adds the display menu item to the standard **Settings** menu |
| `DocentricAXGameCornerShowcaseUser_DCGC` | Security privilege | Grants access to the showcase window |
| `DocentricGameCornerShowcase_en-US` | Label file | The `@DocentricGameCornerShowcase` labels; the window title and help text are reused from the product's `@DocentricAX_GameCorner` labels |

To add the control on a form of your own, copy the form: place `DocGameCornerControl`, size it, and optionally set `initialGame`.

## License

This software is licensed under the **MIT License**. In short: you may use, copy, modify, merge, publish, distribute, sublicense and sell copies of it free of charge, as long as the copyright notice and the permission notice stay in all copies. The software is provided "as is", without warranty of any kind, and the authors are not liable for any claim or damage that arises from it. The full text is in the [LICENSE](LICENSE) file.