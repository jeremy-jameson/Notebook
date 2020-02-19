# WSUS

Saturday, October 05, 2013
8:06 AM

Computers

- All Computers
  - Unassigned Computers
  - .NET Framework 3.5
  - .NET Framework 4
  - .NET Framework 4 Client Profile
  - Fabrikam
  - Internet Explorer 10
  - Internet Explorer 7
  - Internet Explorer 8
  - Internet Explorer 9
  - Silverlight
  - Technology Toolbox
    - WSUS Servers

Synchronization Schedule

- Synchronize automatically
  - First synchronization: **11:10:00 PM**
  - Synchronizations per day: **1**

Automatic Approvals

- Update Rules
  - Default Automatic Approval Rule
    - When an update is in **Critical Updates, Definition Updates, Security Updates**
    - Approve the update for **all computers**
- Advanced
  - WSUS Updates
    - Automatically approve updates to the WSUS product itself
  - Revisions to Updates
    - Automatically approve new revisions of updates that are already approved
      - Automatically decline updates when a new revision causes them to expire

E-Mail Notifications

- General
  - Send status reports
    - Frequency: **Daily**
    - Send reports at: **5:00:00 AM**
    - Recipients: **jjameson@technologytoolbox.com**
  - Language: **English**
- E-Mail Server
  - Server Information
    - Outgoing e-mail server (SMTP): **smtp.technologytoolbox.com**
    - Port number: **25**
  - Sender Information
    - Sender name: **svc-wsus@technologytoolbox.com**
    - E-mail address: **svc-wsus@technologytoolbox.com**
