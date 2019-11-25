![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 在AlwaysOn中查询主服务器
#### Query The Primary Server In AlwaysOn
**发布-日期: 2016年8月17日 (评论)**


## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
以下是另一种你可以快速查询主服务器的便捷方式。我之前确实发布过这个主题的帖子，但那次是用的精确结果（precise result）。如果你想查看涉及的所有服务器，可以使用这个小数字。

它只需查看
`sys.dm_hadr_availability_replica_states`中的`[ROLE_DESC]`值即可。

简单地说......如果PRIMARY被列了出来，那么你就在主服务器上，因为主服务器将显示AlwaysOn配置中涉及的服务器。

如果是从辅助服务器运行的，那么`[ROLE_DESC]`中的PRIMARY值则不会被列出来。这是确定哪个服务器是哪个的简单方法。在任何程序执行之前，我会将这个与某些自动化逻辑一起用作快速检查。

这个特殊的例子是个旧的Sharepoint 2013环境，它是通过SQL Server 2014使用的Server 2012环境的操作系统。有点古老，但提到它并没有什么坏处。



## English
Here’s another quick way you can query the primary server. I’ve posted about this before sure; but that was using a precise result. If you wanted to see all servers involved you can use this little number. 

It works by simply looking at the `[ROLE_DESC]` value from `sys.dm_hadr_availability_replica_states`. Simply put… If the PRIMARY is listed, then you are on the Primary server because the Primary will show the servers involved in the AlwaysOn configuration.

If this is run from the Secondary server then the PRIMARY value from the `[ROLE_DESC]` will not be listed. 

It’s a simple method to determine which server is which. I use this with certain automation logic as a quick check before any processes are carried out. This particular example is of an old Sharepoint 2013 environment which is using an OS of Server 2012 environment with SQL Server 2014. Ancient at this point but doesn’t hurt to mention it.


---
## Logic
```SQL
use master;
set nocount on
set ansi_warnings off
 
select * from sys.dm_hadr_availability_replica_states;
 
if exists(select role_desc from sys.dm_hadr_availability_replica_states where role_desc in ('primary'))
    begin
        print 'The Primary role was found.'
    end
    else
        print 'The Primary role was NOT found.'


```

![#](images/query-the-primary-serverin-sql-alwayson-a.png?raw=true "#")

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

