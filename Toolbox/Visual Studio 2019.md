# Visual Studio 2019

Wednesday, April 3, 2019
4:25 AM

## Download Visual Studio 2019

### Reference

**Create a network installation of Visual Studio**\
From <[https://docs.microsoft.com/en-us/visualstudio/install/create-a-network-installation-of-visual-studio?view=vs-2019](https://docs.microsoft.com/en-us/visualstudio/install/create-a-network-installation-of-visual-studio?view=vs-2019)>

### Initial (2019-04-03)

```PowerShell
& "C:\Users\jjameson\Downloads\vs_enterprise__884083982.1552908495.exe" `
    --layout 'C:\vs2019\Enterprise' --includeOptional --lang en-US

robocopy C:\vs2019\ '\\TT-FS01\Products\Microsoft\Visual Studio 2019' /E /MIR
```
