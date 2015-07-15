# OPB's - Other People's Bugs

Sunday, April 07, 2013
8:26 AM

**[Bug 857672](Bug 857672)** - Address Bar not working\
Pasted from <[https://bugzilla.mozilla.org/show_bug.cgi?id=857672](https://bugzilla.mozilla.org/show_bug.cgi?id=857672)>

[http://support.mozilla.org/en-US/questions/955424](http://support.mozilla.org/en-US/questions/955424)

**Problem getting bootstrap.css to be included using wiredep**\
From <[https://github.com/yeoman/generator-angular/issues/1116](https://github.com/yeoman/generator-angular/issues/1116)>

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
