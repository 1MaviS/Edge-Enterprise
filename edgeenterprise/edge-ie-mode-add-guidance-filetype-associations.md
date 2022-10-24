---
title: "Associate file extensions with Internet Explorer mode"
ms.author: shisub
author: dan-wesley
manager: srugh
ms.date: 10/24/2022
audience: ITPro
ms.topic: conceptual
ms.prod: microsoft-edge
ms.localizationpriority: medium
ms.collection: M365-modern-desktop
description: "Associate file extensions with Internet Explorer mode"
---

# Associate file extensions with Internet Explorer mode

>[!Note]
> The retired, out-of-support Internet Explorer 11 (IE11) desktop application will be permanently disabled on certain versions of Windows 10 as part of the February 2023 Windows security update (“B”) release scheduled for February 14, 2023. [Learn more](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/internet-explorer-11-desktop-app-retirement-faq/ba-p/2366549)  
>
> We highly recommend setting up IE mode in Microsoft Edge and disabling IE11 prior to this date to ensure your organization does not experience business disruption. [Learn how](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/control-ie-retirement-on-your-own-schedule-with-the-disable-ie/ba-p/3627725)
>

This article explains how to associate Microsoft Edge with Internet Explorer mode with file extensions for desktop applications.

> [!NOTE]
> This article applies to Microsoft Edge version 86 or later.

## Guidance for file extension association with Internet Explorer mode

The following instructions show an entry that associates Microsoft Edge with IE mode with the \.mht file type. Use the following steps as a guide for setting a file association.

> [!NOTE]
> You can set specific file extensions to open in Internet Explorer mode by default using the policy to **Set a default associations configuration file**. For more information, see [Policy CSP - ApplicationDefaults](/windows/client-management/mdm/policy-csp-applicationdefaults#applicationdefaults-defaultassociationsconfiguration).

1. Define a new ProgID with the Microsoft Edge channel to use to open with Internet Explorer mode. The ProgID includes the application name and Icon and the full path to msedge.exe.

```markdown
[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\Application]
"ApplicationCompany"="Microsoft Corporation"
"ApplicationName"="Microsoft Edge with IE Mode"
"ApplicationIcon"="C:\\<edge_installation_dir>\\msedge.exe,0"
"AppUserModelId"=""
```

```markdown
[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\DefaultIcon]
@="C:\\<edge_installation_dir>\\msedge.exe,4"
```

2. Configure shell updates to pass the command line needed to open with IE mode.

```markdown
[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\shell\open\command]
@="\"C:\\<edge_installation_dir>\\msedge.exe\" -ie-mode-file-url -- \"%1\""
```

3. Finally, associate the \.mht file extension with a new ProgID. Add your ProgID as a value name, with the value type of REG_SZ.

```markdown
[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.mht\OpenWithProgids]
"MSEdgeIEModeMHT"=hex(0):
```

After you set the keys described in the previous example, your users will see another option on the **Open with** menu to open an \.mht file using Microsoft Edge \<channel\> with IE mode.

## Registry Example

You can save the following code snippet as a .reg file and import it into the registry.

```markdown
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.mht\OpenWithProgids]
"MSEdgeIEModeMHT"=hex(0):

[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT]

[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\Application]
"ApplicationCompany"="Microsoft Corporation"
"ApplicationName"="Microsoft Edge with IE Mode"
"ApplicationIcon"="C:\\<edge_installation_dir>\\msedge.exe,0"
"AppUserModelId"=""

[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\DefaultIcon]
@="C:\\<edge_installation_dir>\\msedge.exe,4"

[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\shell]

[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\shell\open]

[HKEY_CURRENT_USER\SOFTWARE\Classes\MSEdgeIEModeMHT\shell\open\command]
@="\"C:\\<edge_installation_dir>\\msedge.exe\" -ie-mode-file-url -- \"%1\""

```

## Configuring file types to open in Internet Explorer mode

Starting with Microsoft Edge 88, you can configure specific file type links to open in Internet Explorer mode using the policy [Show context menu to open links in Internet Explorer mode](./microsoft-edge-policies.md#internetexplorerintegrationreloadiniemodeallowed).

You can define file types this option should apply to, by specifying file extensions in this policy [Open local files in Internet Explorer mode file extension allow list](./microsoft-edge-policies.md#internetexplorerintegrationlocalfileextensionallowlist). 

## See also

- [About IE mode](./edge-ie-mode.md)
- [Configurable sites information](./edge-learnmore-configurable-sites-ie-mode.md)
- [Additional Enterprise Mode information](/internet-explorer/ie11-deploy-guide/enterprise-mode-overview-for-ie11)
- [Setting file type associations](/windows/win32/shell/fa-file-types)
- [Microsoft Edge Enterprise landing page](https://aka.ms/EdgeEnterprise)