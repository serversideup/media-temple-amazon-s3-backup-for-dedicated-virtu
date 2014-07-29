# Automating the Process
We have made it this far, but it would be really pointless if this was not automated. Fortunately, Linux allows us to run a script every hour to check if there is a new backup. If there is a backup, then it will then execute the move to Amazon S3. Our script will be located in ``/etc/cron.hourly/``. Create the file using ``nano`` and **be sure to NOT have .sh at the end of the filename** (otherwise it will not execute).

```
nano /etc/cron.hourly/aws-s3move
```

Here is the code that we can use to check our directory ([online version here](https://gist.github.com/jaydrogers/9963865#file-amazons3move-full-sh)):

```sh
#! /bin/bash
# Author: Jay Rogers - jay@521dimensions.com

#IMPORTANT -- CONFIGURATION VARIABLES
BACKUPDOMAIN=backup.mydomain.com
SERVERBUCKETNAME=myserver-pleskfull

#Check to see if there are any SERVER backups to move. If so, move the backups to Amazon S3
if [ "$(find /var/www/vhosts/$BACKUPDOMAIN/s3backups/fullserver/ -name "*.tar")" ]; then
        #Make sure that CRON is able to find the AWS Configuration File
        export AWS_CONFIG_FILE=/root/.aws/config
        #Execute S3 moved to Bucket configured above
        aws s3 mv /var/www/vhosts/$BACKUPDOMAIN/s3backups/fullserver/ s3://$SERVERBUCKETNAME --recursive
        exit 0
else
    	#If nothing exists in any of these folders, then do nothing
    	exit 0
fi
```

Write the file out with ``nano`` and then set the file permissions so our script can be executed:

```
chmod 755 /etc/cron.hourly/aws-s3move
```
Since our script is now executable, it is now ready for a test run. **Run another backup** (like what we did before — just the server configuration). Only this time when it completes, we will run our script. When you run it via BASH, you should see the results below:

```
[root@myserver ~]# bash /etc/cron.hourly/aws-s3move

move: ../var/www/vhosts/backup.mysite.com/s3backups/fullserver/test_1404031716.tar to s3://myserver-pleskfull/test_1404031716.tar
```

