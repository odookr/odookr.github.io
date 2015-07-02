**Database Auto Backup** 은 시간 간격에 따라 자동으로 백업할 수 있도록 하고 있다.

[Automatic backup for Odoo Database](http://bistasolutions.com/blogs/2014/11/27/how-to-take-automated-backups-of-you-odoo-database/)

## 모듈 설정:

1) 모듈을 설치하고 나서 스케쥴러를 작성해야 한다:
설정(여기서 Ver.8.0) 'Settings > Automation > Scheduled Actions' 에서 다음을 설정한다.

Odoo Implementation

Following things needs to be taken care while configuring the scheduler:

- Interval Number: It indicates interval time between the next backup schedule run.

- Interval Unit: It works in combination with the Interval number as it contains the unit for interval for eg. In the above snapshot the interval time is set for 5 minutes.

- Next Execution Date: It carries the next run time along with the dates, While configuring for the first time we have to set the time from here.

-- Number of Calls: It indicates the number of times the schedule action will happen, Its recommended to keep a negative value (eg. -1), so that it runs infinite times without being stopped.

-- Repeat Missed checkbox needs to be kept unchecked.

-- Under Technical Data tab, set the value for ‘Object’ to ‘db.backup’ and ‘Function’ to ‘schedule_backup’.


2) Configure Backup:
Go to Settings > Configure Backup (as shown below).

Odoo Customisation

Following things needs to be taken care while configuring the Backup:

i) Host: It should be set to localhost or the appropriate host name.

ii) Database: Mention the name of the database which needs to be backup.

iii) Port: Mention the appropriate port number.

iv) Backup Directory: Mention the location of the directory where the backup files needs to be stored on the system.

Note: Make sure the destination directory has full rights, to avoid permission issues. Once all the configuration is done, the backup will be executed at its scheduled time and a .sql file would be available at the mentioned location.

