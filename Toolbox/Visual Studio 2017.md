# Visual Studio 2017

Wednesday, March 8, 2017
10:05 AM

**Create an offline installer for Visual Studio 2017**\
From <[https://docs.microsoft.com/en-us/visualstudio/install/create-an-offline-installation-of-visual-studio](https://docs.microsoft.com/en-us/visualstudio/install/create-an-offline-installation-of-visual-studio)>

## Original (2017-03-08)

```PowerShell
& "C:\Users\jjameson\Downloads\mu_visual_studio_enterprise_2017_x86_x64_10049783.exe" `
    --layout '\\TT-FS01\Products\Microsoft\Visual Studio 2017\Enterprise' --lang en-US
```

## Update (2017-03-27)

```PowerShell
& "C:\Users\jjameson\Downloads\vs_enterprise__1167797576.1490649074.exe" `
    --layout '\\TT-FS01\Products\Microsoft\Visual Studio 2017\Enterprise' --lang en-US
```

## Update (2017-07-09)

```PowerShell
& "C:\Users\jjameson\Downloads\vs_Enterprise.exe" `
    --layout 'C:\vs2017\Enterprise' --includeOptional --lang en-US
```
