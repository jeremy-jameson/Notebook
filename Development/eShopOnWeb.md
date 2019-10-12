# eShopOnWeb

Thursday, October 3, 2019
5:39 PM

```PowerShell
cls
```

## # Install Azure PowerShell module

```PowerShell
Install-Module -Name Az -AllowClobber -Scope AllUsers
```

```PowerShell
cls
```

## # Configure service connection in Azure DevOps

```PowerShell
Connect-AzAccount
```

> **Note**
> 
> Login as **jjameson-admin@technologytoolbox.com**

```PowerShell
cls
```

## # Select subscription

```PowerShell
Select-AzSubscription -Subscription "Visual Studio Ultimate with MSDN"
```

```PowerShell
cls
```

## # Create resource groups

```PowerShell
New-AzResourceGroup -Location 'East US' -Name eShopOnWeb-001-dev
New-AzResourceGroup -Location 'East US' -Name eShopOnWeb-001-test
New-AzResourceGroup -Location 'East US' -Name eShopOnWeb-001
```

```PowerShell
cls
```

## # Create Service Principal for deploying through Azure DevOps release pipeline

### # Create Service Principal and save result for retrieving password

```PowerShell
$servicePrincipal = New-AzADServicePrincipal `
    -DisplayName techtoolbox-eShopOnWeb-deployment
```

### # Store password for Service Principal

```PowerShell
$bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR(
    $servicePrincipal.Secret)

$unsecureSecret = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)

$unsecureSecret

{redacted}
```

### Record Service Principal password in secure location

```PowerShell
cls
```

## # Assign permissions to resource groups

```PowerShell
New-AzRoleAssignment `
    -ApplicationId $servicePrincipal.ApplicationId `
    -ResourceGroupName eShopOnWeb-001-dev `
    -RoleDefinitionName Contributor

New-AzRoleAssignment `
    -ApplicationId $servicePrincipal.ApplicationId `
    -ResourceGroupName eShopOnWeb-001-test `
    -RoleDefinitionName Contributor

New-AzRoleAssignment `
    -ApplicationId $servicePrincipal.ApplicationId `
    -ResourceGroupName eShopOnWeb-001 `
    -RoleDefinitionName Contributor
```

## Add service connection for Azure Resource Manager to Azure DevOps project

![(screenshot)](https://assets.technologytoolbox.com/screenshots/05/B52BFC1D22BF99191390D847FED2BA8F6A829F05.png)

```PowerShell
cls
```

## # Rebuild DEV

### # Delete resource group

```PowerShell
Remove-AzResourceGroup -Name eShopOnWeb-001-dev -Force
```

```PowerShell
cls
```

### # Create resource group

```PowerShell
New-AzResourceGroup -Location 'East US' -Name eShopOnWeb-01-dev
```

```PowerShell
cls
```

### # Assign permissions to resource group

```PowerShell
$servicePrincipal = Get-AzADServicePrincipal -DisplayName techtoolbox-eShopOnWeb-deployment

New-AzRoleAssignment `
    -ApplicationId $servicePrincipal.ApplicationId `
    -ResourceGroupName eShopOnWeb-001-dev `
    -RoleDefinitionName Contributor
```

## Create release pipeline

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A0/E124BC1DFEF0011709F2033893310780DDBDECA0.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E5/FC99521E3DCBB8E4EE5622DEBC9F505D381E99E5.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/76/2EDDC5026380FD4A49FF6556D59DC281C203E476.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9D/E04A1B674BDE9E6946235247DE59C0A032EF5C9D.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/4F/3F8133E8E34D96A9F82DCD947C591AE919808E4F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2E/93512CC1B45880F9DBEA4C7E6F31720ED9F3E02E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/59/04C20A814165793FFF231D078EF8432CADFDED59.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/60/0F2C43F429AA3AAEFB40F9656A314EBF32B48960.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/81/C075345D0AA06895406D8169889C088B412A8D81.png)

```
Click Authorize
```

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B4/3DEB247E820E8067A6FA059B9D7C83CF2D428BB4.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/06/78843BA12CF729F856B071AA219CF48290F06A06.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0D/67BD6CF92377EE0761B060B80A67CC9BF40D590D.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9A/725AE7DF5D909393704130AA27627260C2AD899A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/91/2EF54343BF8BFC31F4BB27F44B1CEED57973A891.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/29/91587B09CDC9082E439C1EE6698580C114DFF629.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BA/28EE2CAB8A0FCAAEC329E71153E716703D6EF8BA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1A/ECA9C9F6D4DAD3645E0A8FF0476F48E9659CBE1A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/99/D3E040EEAAF71E1EF2EC8B2450806731110F0F99.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E8/2E14E7674609F83E89F7CD060A6E0B2F330E40E8.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/4A/FD3AACDFF1E2A8B7354E0DCE8E0ECB4D8F1EBD4A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/3B/0F6724532F5EB7792ADC226838FEB390FAE80B3B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/45/819C5F87893651B570C386F60E089B274A089845.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/13/D6DADF29203964FBACF09EFA5DB84AEDB7D8BB13.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/95/759F2249CA50839EF6F5F3EA1951213837EE7F95.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5C/4933972A065740C6500930238B8B01DF7D551E5C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D4/ECF89830C367F9B279F1FDC3E7CF42BE600700D4.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/4C/672ADD08D8172CC1DBD7168D241B95BDFAF7534C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/06/E04A32AC8A6C8417D25F8CAB8A3807F4E11EAD06.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/94/96F9881A885FEF7046F488671E9A6DC659C54894.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/77/CE4CBA3EE15A333B07F39560963A0D0F64993377.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/21/EE78DB9AFFA3A91367243661B8CE5700600AA921.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AB/F701CDEF6AA3027A90ACAFEB5C0F2372074885AB.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/59/C3739431B5FEAE6954422B342F453E77FBBAA759.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F3/570A7850218204B8F7804C408B3E3BB5DF7621F3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/98/CCB936C903EC2FC43436EE4A8E95EC7B48C2AD98.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2C/2896833FB2D89F8FFCD92ABE41A4BDE1C7E34A2C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/99/14C05E1DE5792E5BDA106ADC04CEF34F6FD5DD99.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F1/E50C72D3F22D6B1E856722F7C4E333E315DE43F1.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D1/BDDD7C1AB3F1410AFCC326D518A79DE5C39D08D1.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0A/0EFFE56F6C9FA651735EF88B2BE81D7F1875D30A.png)
