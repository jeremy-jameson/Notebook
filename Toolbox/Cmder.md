# Cmder

Monday, October 8, 2018
1:43 PM

Settings

- General
  - General settings
    - Startup task: **{PowerShell::PowerShell}**
  - Fonts
    - Main console font
      - Size: **14**
  - Size & Pos
    - Console buffer height
      - Length of backscroll buffer: **3000**
- Startup
  - Tasks
    - {PowerShell::PowerShell as Administrator}
      - Remove "-NoProfile" from command window
    - {PowerShell::PowerShell}
      - Remove "-NoProfile" from command window
    - Custom tasks
      - **VS2015**
        - **cmd /k "%VS140COMNTOOLS%VsDevCmd.bat" -new_console:d:"C:\\NotBackedUp":t:"VS2015"**
      - **VS2017**
        - **cmd /k "C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\Enterprise\\Common7\\Tools\\VsDevCmd.bat"  -new_console:d:"C:\\NotBackedUp":t:"VS2017"**
- Features
  - Transparency
    - Active window transparency: **disabled (unchecked)**

```PowerShell
cd C:\NotBackedUp\Temp

git clone https://github.com/martinlindhe/base16-conemu.git

cd base16-conemu

.\Install-ConEmuTheme.ps1 `
    -ConfigPath "C:\NotBackedUp\Public\Toolbox\cmder\vendor\conemu-maximus5\ConEmu.xml" `
    -Operation Add `
    -ThemePathOrName .\themes\base16-default-dark.xml
```

Settings

- Features
  - Colors
    - Scheme: **Base16 Default Dark**
    - Set color 0: #3b3b3b ("Black")
    - Set color 1/4: #1e4173 ("DarkBlue")
    - Set color 2: #3b8c1a ("DarkGreen")
    - Set color 3/6: #aab9cd ("DarkCyan")
    - Set color 4/1: #bd1c1c ("DarkRed")
    - Set color 5: #926387 ("DarkMagenta")
    - Set color 6/3: #ffff99 ("DarkYellow")
    - Set color 7: #dddddd ("Gray")
    - Set color 8: #959595 ("DarkGray")
    - Set color 9: #3c78c3 ("Blue")
    - Set color 10: #77c856 ("Green")
    - Set color 11: #c8d7eb ("Cyan")
    - Set color 12: #ff9999 ("Red")
    - Set color 13: #ba8baf ("Magenta")
    - Set color 14: #ffffcc ("Yellow")
    - Set color 15: #ffffff ("White")
    - Save scheme as **Technology Toolbox Dark**

```Console
Notepad C:\NotBackedUp\Public\Toolbox\cmder\vendor\profile.ps1

108:    Microsoft.PowerShell.Utility\Write-Host $pwd.ProviderPath -NoNewLine -ForegroundColor Green DarkGray
```
