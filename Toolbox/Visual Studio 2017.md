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

**Visual Studio 2017 Professional/Enterprise Layout installation fails**\
From <[https://developercommunity.visualstudio.com/solutions/79186/view.html](https://developercommunity.visualstudio.com/solutions/79186/view.html)>

```PowerShell
& "C:\Users\jjameson\Downloads\vs_Enterprise.exe" `
    --layout 'C:\vs2017\Enterprise' --includeOptional --lang en-US
```

## Update (2018-10-04)

```PowerShell
& "C:\Users\jjameson\Downloads\vs_enterprise__1707846514.1538150767.exe" `
    --layout 'C:\vs2017\Enterprise' --includeOptional --lang en-US

robocopy C:\vs2017\ '\\TT-FS01\Products\Microsoft\Visual Studio 2017' /E /MIR
```

## Update (2018-12-224)

```PowerShell
& "C:\Users\jjameson\Downloads\vs_enterprise__740322565.1545343127.exe" `
    --layout 'C:\vs2017\Enterprise' --includeOptional --lang en-US

robocopy C:\vs2017\ '\\TT-FS01\Products\Microsoft\Visual Studio 2017' /E /MIR
```
