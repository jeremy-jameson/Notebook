# NETLOGON 5719 errors

Tuesday, April 30, 2013
11:30 AM

Log Name:      System\
Source:        NETLOGON\
Date:          4/30/2013 11:27:52 AM\
Event ID:      5719\
Task Category: None\
Level:         Error\
Keywords:      Classic\
User:          N/A\
Computer:      foobar9.corp.technologytoolbox.com\
Description:\
This computer was not able to set up a secure session with a domain controller in domain TECHTOOLBOX due to the following:\
There are currently no logon servers available to service the logon request.\
This may lead to authentication problems. Make sure that this computer is connected to the network. If the problem persists, please contact your domain administrator.

ADDITIONAL INFO\
If this computer is a domain controller for the specified domain, it sets up the secure session to the primary domain controller emulator in the specified domain. Otherwise, this computer sets up the secure session to any domain controller in the specified domain.\
Event Xml:\
<Event xmlns="[http://schemas.microsoft.com/win/2004/08/events/event](http://schemas.microsoft.com/win/2004/08/events/event)">\
  `<System>`\
    `<Provider Name="NETLOGON" />`\
    `<EventID Qualifiers="0">`5719`</EventID>`\
    `<Level>`2`</Level>`\
    `<Task>`0`</Task>`\
    `<Keywords>`0x80000000000000`</Keywords>`\
    `<TimeCreated SystemTime="2013-04-30T17:27:52.000000000Z" />`\
    `<EventRecordID>`3455`</EventRecordID>`\
    `<Channel>`System`</Channel>`\
    `<Computer>`foobar9.corp.technologytoolbox.com`</Computer>`\
    `<Security />`\
  `</System>`\
  `<EventData>`\
    `<Data>`TECHTOOLBOX`</Data>`\
    `<Data>`%%1311`</Data>`\
    `<Binary>`5E0000C0`</Binary>`\
  `</EventData>`\
`</Event>`
