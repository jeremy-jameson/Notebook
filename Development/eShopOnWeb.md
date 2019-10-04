# eShopOnWeb

Thursday, October 3, 2019
5:39 PM

```PowerShell
cls
# Install Azure CLI

Push-Location C:\NotBackedUp\Temp

Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi

Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'

Pop-Location

# Configure service connection in Azure DevOps

az login

Note: Login as jjameson-admin@technologytoolbox.com
```

```PowerShell
cls
# Select subscription

az account set --subscription "Visual Studio Ultimate with MSDN"

# Create resource groups

az group create --location eastus --name eShopOnWeb-001-dev
az group create --location eastus --name eShopOnWeb-001-test
az group create --location eastus --name eShopOnWeb-001
```

```PowerShell
cls

$subscriptionId = "********-fdf5-4fd0-b21b-{redacted}"

az ad sp create-for-rbac --name "http://sp-eShopOnWeb-001-deployment" `
    --role contributor `
    --scopes /subscriptions/$subscriptionId/resourceGroups/eShopOnWeb-001-dev `
        /subscriptions/$subscriptionId/resourceGroups/eShopOnWeb-001-test `
        /subscriptions/$subscriptionId/resourceGroups/eShopOnWeb-001

{
  "appId": "da48546d-9435-430f-b225-{redacted}",
  "displayName": "sp-eShopOnWeb-001-deployment",
  "name": "http://sp-eShopOnWeb-001-deployment",
  "password": "5a5939ed-d287-4877-8496-{redacted}",
  "tenant": "********-b76f-400c-ba19-{redacted}"
}
```

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0E/1F5E1B64BE23C62F0E6542BC7BD4FBEBE749730E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B6/D5F9551B1162FDB986EE2BDF3AF2BB87125D56B6.png)

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

![(screenshot)](https://assets.technologytoolbox.com/screenshots/06/E04A32AC8A6C8417D25F8CAB8A3807F4E11EAD06.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/94/96F9881A885FEF7046F488671E9A6DC659C54894.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/77/CE4CBA3EE15A333B07F39560963A0D0F64993377.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/21/EE78DB9AFFA3A91367243661B8CE5700600AA921.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AB/F701CDEF6AA3027A90ACAFEB5C0F2372074885AB.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/59/C3739431B5FEAE6954422B342F453E77FBBAA759.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1F/3F2EBAC9856C5F47F643961F657F0CF21757BB1F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F3/570A7850218204B8F7804C408B3E3BB5DF7621F3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/98/CCB936C903EC2FC43436EE4A8E95EC7B48C2AD98.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2C/2896833FB2D89F8FFCD92ABE41A4BDE1C7E34A2C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/99/14C05E1DE5792E5BDA106ADC04CEF34F6FD5DD99.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F1/E50C72D3F22D6B1E856722F7C4E333E315DE43F1.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D1/BDDD7C1AB3F1410AFCC326D518A79DE5C39D08D1.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0A/0EFFE56F6C9FA651735EF88B2BE81D7F1875D30A.png)
