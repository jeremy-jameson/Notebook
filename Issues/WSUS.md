# WSUS

Saturday, January 12, 2019
9:26 AM

Slow cleanup

[https://www.google.com/search?client=firefox-b-1-ab&q=spdeleteupdate+slow](https://www.google.com/search?client=firefox-b-1-ab&q=spdeleteupdate+slow)

**Enhancing WSUS database cleanup performance SQL script**\
From <[https://stevethompsonmvp.wordpress.com/2018/05/01/enhancing-wsus-database-cleanup-performance-sql-script/](https://stevethompsonmvp.wordpress.com/2018/05/01/enhancing-wsus-database-cleanup-performance-sql-script/)>

**WSUS Delete Obsolete Updates**\
From <[https://kickthatcomputer.wordpress.com/2017/08/15/wsus-delete-obsolete-updates/](https://kickthatcomputer.wordpress.com/2017/08/15/wsus-delete-obsolete-updates/)>

**SCCM/WSUS – Clean-up WSUS Strategy & Maintenance**\
From <[https://www.walshamsolutions.com/SCCM-WSUS-Cleanup-WSUS-Strategy-maintenance](https://www.walshamsolutions.com/SCCM-WSUS-Cleanup-WSUS-Strategy-maintenance)>

**Cleaning a Stuck WSUS Server: Too Many Unapproved Updates**\
From <[http://pnijjar.freeshell.org/2016/wsus-stuck/](http://pnijjar.freeshell.org/2016/wsus-stuck/)>

USE [SUSDB]\
GO\
CREATE NONCLUSTERED INDEX [IX_tbRevisionSupersedesUpdate] ON [dbo].[tbRevisionSupersedesUpdate]([SupersededUpdateID])\
GO\
CREATE NONCLUSTERED INDEX [IX_tbLocalizedPropertyForRevision] ON [dbo].[tbLocalizedPropertyForRevision]([LocalizedPropertyID])\
GO


