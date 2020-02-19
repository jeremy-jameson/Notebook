# Backups

Wednesday, January 01, 2014
5:24 PM

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C5/39B76A472B8949D317AD9305B1222E0F714D36C5.png)

**Hyper-V and Windows Server Backup - getting it to run fast**\
Pasted from <[http://www.backupassist.com/blog/support/hyper-v-and-windows-server-backup-getting-it-to-run-fast/](http://www.backupassist.com/blog/support/hyper-v-and-windows-server-backup-getting-it-to-run-fast/)>

## Add backup disk

```PowerShell
$vmName = "BEAST-TEST"

Stop-VM $vmName

$vhdPath = "C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\"
    + $vmName + "_Backup01.vhdx"

New-VHD -Path $vhdPath -SizeBytes 30GB -Dynamic

Add-VMHardDiskDrive -VMName $vmName -Path $vhdPath

Start-VM $vmName
```

![(screenshot)](https://assets.technologytoolbox.com/screenshots/47/8D05B4A8FC06D75E616CC45066288342B5E6C247.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B8/29131AA6EDE5CBFC5A52427B6154BCCE839750B8.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F7/648041F0F1B80B911094614C0DB5D2979B2175F7.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EA/ECCBC273A73ECDA63BACC96AA38C4A0DEA4DE6EA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DE/6FC32A0FA9A98AD87474E3643C5A007A02E08DDE.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/6E/3EBADF465C6E833DB224EC0E4974D9CCC739146E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BF/A66540E58FEA987C4656874C77EDCD5F6795FCBF.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/23/FD1EE09A1F57F464453F41DF30C48B4094034D23.png)

## Enable iSCSI Initiator

Start -> iSCSI Initiator

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DF/3F6DE64EBF1E201BDD47D798802EEBB2C9A72CDF.png)

Click **Yes**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5B/38F75FA3835C75B20A80D63314FE3463D9B3095B.png)

## Discover iSCSI Target portal

On the **Discovery** tab, in the **Target portals** section, click **Discover Portal...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/01/5AF2023C517FB8FDD3FACC7214E310797B6A0401.png)

In the **Discover Target Portal** window, in the **IP address or DNS name** box, type **10.1.10.106**, and then click **OK**.

## Create iSCSI virtual disk (BEAST-TEST-Backup02.vhdx)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/01/686CE825AC91BDF99951DBCD8F7690F371546301.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CA/57D3BA42B812EEC76A08AAC0451AC32A5E6B55CA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5B/6E99A1A7AF1A0EB079919DD634BFE237860B425B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D6/BC2BF480D916A413C35BDCB9D96286E3238854D6.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D3/182135FBF9FC45A3171C0E0621ADF9ADAB733AD3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F9/551ED6B2353BA8DA0BB0EA94E461B00FC07A63F9.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7B/7C16326C69DB8503E15E35E07C2653E4B4C6837B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CF/D31CA49193A851006C725ABFB1D758FB2F12C6CF.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/52/6D243AE036DA975AB6A09811B123439AF4C8B352.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/77/F6880A73C5EDB0D817100D01E3575E7C5A3EB577.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/4C/CBE127A313427057ED5A31303D0BB8212F1C024C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EC/D5B7854904A97CBEC76E0BD052AEBDF4BA0753EC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C8/097C10BAAAA71AC592CA6F2749209056801657C8.png)

Wait for iSCSI virtual disk to be cleared.

## Configure CHAP on iSCSI Target portal

```PowerShell
Remove-IscsiTargetPortal -TargetPortalAddress 10.1.10.106 -Confirm

$chapUserName =
    "iqn.1991-05.com.microsoft:beast-test.corp.technologytoolbox.com"

New-IscsiTargetPortal -TargetPortalAddress 10.1.10.106 `
    -AuthenticationType OneWayCHAP `
    -ChapUserName $chapUserName -ChapSecret {password}
```

## Configure CHAP on iSCSI Target

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DA/6029B939D5BA8F88098F2F9B8B425516CC35FADA.png)

Click **Connect**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/77/50DC77E45AA28D3BFF48AB02FBDF90144F668077.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C2/3EF7AB9D2670CE32C6F9E92B676C59D3AEBC21C2.png)

On the **Volumes and Devices** tab, click **Auto Configure**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D3/A34E7F894EE4E7C175929A873C63A657500ED5D3.png)

## Online the iSCSI LUN

![(screenshot)](https://assets.technologytoolbox.com/screenshots/71/0142C1A16F79A621D0AB4AE4931797A53BD93B71.png)

Right click **Disk 2** and then click **Online**.

Right click **Disk 2** and then click **Initialize Disk**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B8/29131AA6EDE5CBFC5A52427B6154BCCE839750B8.png)

## Add Windows Server Backup feature

```PowerShell
Add-WindowsFeature Windows-Server-Backup -IncludeManagementTools
```

## Configure backup

![(screenshot)](https://assets.technologytoolbox.com/screenshots/72/DAD684016272EB43172772D9AA3D376AD8477D72.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/46/CFB300166C9DADBE687AEAE1E535C4859DFD7D46.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2F/447794D00DC121CEEA52D6ABB621CA8A1931FF2F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1C/5DC1614003A2C154462A9E5307063F379A8C791C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/30/93543A5E69AF0C3FB8D7C06B4AB521C66A8AA430.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/8A/76B2744A4FAF0AC451ECE7C9A524CF9C204B3F8A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/76/9AE82625AADF351C5C528ACAEEA0B0C37F022276.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DC/8A19F23482F40332647E26B7E10519FDD9FE94DC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7F/FC355C3088FA367ED026BDE422A52A563C83F77F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/57/4C743ADFC3B611610FC8E74908CC63D419B83757.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/8F/FC290D776C4FCEF3F74FDC5555F7C33A9695F38F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A1/018655B85C749E22AEDFD1626868CFE412DFC5A1.png)

## Backup server using scheduled backup options

In the **Actions** pane, click **Backup Once...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9F/3F748A9AB88CE6B0BCD80D2D7DC9599A5681909F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2B/B3630C0D77E8D82BCF740D851606E4F0DC65762B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/74/1E042D520283C8E5E8440370466DA31C68FDE774.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/3C/AC4F9E620B8F05ADC28A4CC8E549B0549375763C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/02/552057DB75CD9E37002CDC2DB6BB678ABD8ED302.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/40/08BCC8A4D04322A9379F498ABD3A8CEA6F0E7940.png)

## Backup server again

Start another backup

![(screenshot)](https://assets.technologytoolbox.com/screenshots/3D/57A589757FFE6E62000EA6853FB5B35C6544543D.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0B/50F8FA03AE7CCD6D739D8E9EC85C0F6B156B2A0B.png)

**Hyper-V and Windows Server Backup - getting it to run fast**\
Pasted from <[http://www.backupassist.com/blog/support/hyper-v-and-windows-server-backup-getting-it-to-run-fast/](http://www.backupassist.com/blog/support/hyper-v-and-windows-server-backup-getting-it-to-run-fast/)>

## # [ROGUE] Checkpoint VM

```PowerShell
Stop-VM BEAST-TEST
Checkpoint-VM BEAST-TEST
Start-VM BEAST-TEST
```

## Enable shadow copies on BEAST-TEST

1. Open Windows Explorer.
2. Right click **Local Disk (C:)** and then click **Configure Shadow Copies...**
3. When prompted by UAT, click **Yes**.
4. Click **Enable**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/02/C7BA4806B5F067240AA642864C97798B3479B802.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A7/5D5632376281971F8151B4D07480F7610FB0D8A7.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0F/E44FE1CFE45A44AD7C4E50C23881CE9A7807B60F.png)

## Backup server (full backup)

Start another backup

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7D/F27A3B1E1E7F90A8F8CFC00AF853A163EAA8DD7D.png)

## Backup server (incremental backup)

Start another backup

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E3/E45B0B5F22CA1572652A49D255B73FEF660FA3E3.png)

## Restore VM snapshot

```PowerShell
Stop-VM BEAST-TEST
Get-VMSnapshot BEAST-TEST | Restore-VMSnapshot -Confirm:$false
Start-VM BEAST-TEST
```

## Backup server

Start another backup

![(screenshot)](https://assets.technologytoolbox.com/screenshots/4E/DA5F623E39453399C98B40478EB526042BCFF64E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BC/6050BF5B6E170BF36FC7215E95AA76F60FE03FBC.png)

## Backup files on BEAST

Backup the following folders on BEAST to [\\\\ICEMAN\\Temp](\\ICEMAN\Temp):

- C:\\BackedUp
- D:\\

## Add File Server feature to BEAST-TEST

```PowerShell
Add-WindowsFeature FS-FileServer
```

## Create shares for user and profile folders

### Reference

**Deploy Folder Redirection with Offline Files**\
Pasted from <[http://technet.microsoft.com/en-us/library/jj649078.aspx](http://technet.microsoft.com/en-us/library/jj649078.aspx)>

![(screenshot)](https://assets.technologytoolbox.com/screenshots/45/3ACAD3842C10466C1BE55E3F91503C0EE5BEBD45.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/45/A6B0A75BD5B673432BF13DDD696E59084359A545.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E6/2747BA2D156764DE51506DCB2C777B79C40A40E6.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C9/9C1369B373E80BB41D73A5DDAE4240CD64B464C9.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B7/AD96247A392760C8B74A2E04222F0F5429CD81B7.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/61/2121205C5DE76CEF4C9982122EB4BDDDC2426961.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/50/37F690A3981F84AD2DC400E5729966A227084550.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F8/BE3FAC37BA8E6A0CE5C79A08D11B33E0D2405CF8.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9D/A6B78DC2A2A554469DC4D7411D186EDD9E1D0E9D.png)

Click **Close**.

Repeat the steps above to create the **Profiles\$** share.

## Recover files

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E7/B053F12FC6F86047D04EED29F417EB3D11A00BE7.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/12/44DDCBCFB3A51A7CAC5C7160B4EE57C44999A312.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/89/6E4FF6B7768CED07EFC87FD20A245772310F9189.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CD/90191E2AD52A7F1C959149A00BB00900B480B5CD.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2C/2AB3D9C404ED173984A948899886BC2AA82E9C2C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/77/A8A853D770905876DD8CC706E30496D6069B4077.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F8/9EB9487AAE149397D09C89C3722D3A43AE3DDFF8.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C5/62114BDC9EDDA92DCBB1DECF35300ECC55B36BC5.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B0/F9F3713A3CDE6CD2AA906988FCD8B2B6EA6158B0.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A5/EC00E72FF65E533C7E0E0BB8AB92EFF9767FC0A5.png)

Repeat the steps above to recover the "Profiles" folders.
