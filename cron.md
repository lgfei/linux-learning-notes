# crond服务启动与关闭
<pre><code>
service crond status
service crond start
service crond stop
service crond reload
</code></pre>

# 全局配置文件(/etc目录下cron.hourly,cron.daily,cron.weekly,cron.monthly,cron.d五个目录和crontab,cron.deny二个文件)
<pre><code>
ls -l /etc/ | grep -w "cron"

cron.daily是每天执行一次的job
cron.weekly是每个星期执行一次的job
cron.monthly是每月执行一次的job
cron.hourly是每个小时执行一次的job
cron.d是系统自动定期需要做的任务
crontab是设定定时任务执行文件
cron.deny文件就是用于控制不让哪些用户使用Crontab的功能
</code></pre>

# crontab命令
<pre><code>
查看当前用户的定时任务
crontab -l
编辑当前用户的定时任务(保存后会写到/var/spool/cron/root，其中root是当前用户名)
crontab -e
删除当前用的所有任务
crontab -r

格式：
*        *        *    *       *         command
minute   hour    day   month   week      command
分       时      天    月      星期       命令

minute： 表示分钟，可以是从0到59之间的任何整数。
hour：表示小时，可以是从0到23之间的任何整数。
day：表示日期，可以是从1到31之间的任何整数。
month：表示月份，可以是从1到12之间的任何整数。
week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。
command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。

星号（*）：代表每的意思，例如month字段如果是星号，则表示每月都执行该命令操作。
逗号（,）：表示分隔时段的意思，例如，“1,3,5,7,9”。
中杠（-）：表示一个时间范围，例如“2-6”表示“2,3,4,5,6”。
正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。
</code></pre>
