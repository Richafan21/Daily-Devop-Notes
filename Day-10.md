# Service Management

---

## Cronjobs

With Cron, you can schedule tasks in Linux as Cron allows you to automate commands by specifying the date, time, and frequency for execution.  
Once configured, the cron daemon runs the task without human intervention, maing system maintenance efficient and reliable.  

Imagine you are required to run the command `uptime` and redirect the output to the file `/tmp/system-report.txt` every day at 9 pm.  
Booting up your device and running this command everyday manually can be tedius, so a Cron job automates this process.  
This is how you would do it at 9pm manually:  
`uptime >> /tmp/ststem-report.txt`

### Scheduling a Cron Job

To schedule the above task with a cronjob:  
`[richard@fan ~]$ crontab -e`  
This command will open the crontab file in VI editor. At the bottom of the file, add the following Cron job configuration:  
`0 21 * * * uptime >> /tmp/system-report.txt`  

The first five fields specify the schedule:  

- Minute (0): The task runs at the 0th minute.
- Hour (21): The task runs at 9 p.m. (21:00 in 24-hour format).
- Day of Month (*): Every day of the month.
- Month (*): Every month.
- Day of Week (*): Every day of the week.

## Examples of Cron Syntax

Run a job on February 19th at 8:10:

`10 8 19 2 *`  
Run a job on February 19th at 8:10 only if it’s a Monday (where 1 represents Monday):  

`10 8 19 2 1`  
Run a job every day at 8:10:

`10 8 * * *`  
Run a job every minute of every hour on all days:

`* * * * *`  
Run a job every two minutes using step values:

`*/2 * * * *`  

### Listing and Verifying Cron Jobs

You can use the option -l to list all cronjobs for a user:  
`crontab -l`

Run `cat /tmp/system-report.txt` to verify the task ran as expected at 9 pm.
