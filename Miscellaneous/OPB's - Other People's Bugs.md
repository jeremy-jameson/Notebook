# OPB's - Other People's Bugs

Sunday, April 07, 2013
8:26 AM

## Mozilla Firefox

**[Bug 857672](Bug 857672)** - Address Bar not working\
Pasted from <[https://bugzilla.mozilla.org/show_bug.cgi?id=857672](https://bugzilla.mozilla.org/show_bug.cgi?id=857672)>

[http://support.mozilla.org/en-US/questions/955424](http://support.mozilla.org/en-US/questions/955424)

## Yeoman

**Problem getting bootstrap.css to be included using wiredep**\
From <[https://github.com/yeoman/generator-angular/issues/1116](https://github.com/yeoman/generator-angular/issues/1116)>

## Node.js

**npm on windows, install with -g flag should go into appdata/local rather than current appdata/roaming?**\
From <[https://github.com/npm/npm/issues/4564](https://github.com/npm/npm/issues/4564)>

**`npm install -g bower` goes into infinite loop on windows with %appdata% being a UNC path**\
From <[https://github.com/npm/npm/issues/8814](https://github.com/npm/npm/issues/8814)>

**Workaround:**

```Console
notepad "C:\Program Files\nodejs\node_modules\npm\npmrc"
```

In Notepad, change:

```Text
    prefix=${APPDATA}\npm
```

…to:

```Text
    ;prefix=${APPDATA}\npm
    prefix=${LOCALAPPDATA}\npm
    cache=${LOCALAPPDATA}\npm-cache
```

## Visual Studio 2015

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BF/1541379F66011CFCFD77433DF3D55CDBC15C3EBF.png)

## Windows 10

**Microsoft Edge crashes on load after failed windows update**\
From <[https://social.technet.microsoft.com/Forums/en-US/0e84eb26-ebd1-440d-b04f-b53401fa6a6f/microsoft-edge-crashes-on-load-after-failed-windows-update?forum=win10itprogeneral](https://social.technet.microsoft.com/Forums/en-US/0e84eb26-ebd1-440d-b04f-b53401fa6a6f/microsoft-edge-crashes-on-load-after-failed-windows-update?forum=win10itprogeneral)>

**Windows 10 Threshold 2 - Edge/Search issues for domain joined PCs**\
From <[https://social.technet.microsoft.com/Forums/en-US/fd436515-6423-4015-9afe-d7e6034909ab/windows-10-threshold-2-edgesearch-issues-for-domain-joined-pcs?forum=win10itprogeneral](https://social.technet.microsoft.com/Forums/en-US/fd436515-6423-4015-9afe-d7e6034909ab/windows-10-threshold-2-edgesearch-issues-for-domain-joined-pcs?forum=win10itprogeneral)>

**[IE11] and Edge crash on start if customer use Roaming Profile (Win10 1511)**\
From <[https://connect.microsoft.com/IE/feedback/details/2031419/ie11-and-edge-crash-on-start-if-customer-use-roaming-profile-win10-1511](https://connect.microsoft.com/IE/feedback/details/2031419/ie11-and-edge-crash-on-start-if-customer-use-roaming-profile-win10-1511)>

**Win 10 1511 Post Upgrade Issues Start, Cortana, Edge, Action Center, Roaming Profiles**\
From <[https://answers.microsoft.com/en-us/windows/forum/windows_10-performance/win-10-1511-post-upgrade-issues-start-cortana-edge/7d2af4ef-3dda-4261-8b41-471eb343094b?page=5](https://answers.microsoft.com/en-us/windows/forum/windows_10-performance/win-10-1511-post-upgrade-issues-start-cortana-edge/7d2af4ef-3dda-4261-8b41-471eb343094b?page=5)>

**Cortana Network Usage**\
From <[https://answers.microsoft.com/en-us/windows/forum/all/cortana-network-usage/718c251e-82b8-4538-a25b-a3e6cef09aac](https://answers.microsoft.com/en-us/windows/forum/all/cortana-network-usage/718c251e-82b8-4538-a25b-a3e6cef09aac)>
