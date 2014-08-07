---
layout: post
title: "Configure Delayed_job For Rails3.x and Rails4.x"
date: 2014-08-07 13:11:49 +0530
comments: true
categories: 
---

Elastic Beanstalk will execute any script in **/opt/elasticbeanstalk/hooks/appdeploy/post** after the web server is restarted. This means if you drop shell scripts in this directory they will be run. Please also refer the [AWS Documentation](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/customize-containers-windows-ec2.html#customize-containers-windows-format-container_commands) for container_commands.

I have created a file name **delayed_job.config** in the **.ebextensions** folder in order to deploy a shell script that runs after the deployment gets over.

###For RAILS 3.x
---------------------------
Below  is my delayed_job.sh file for **Rails 3.x**
```sh
commands:
  create_post_dir:
    command: "mkdir /opt/elasticbeanstalk/hooks/appdeploy/post"
    ignoreErrors: true
    
files:
  "/opt/elasticbeanstalk/hooks/appdeploy/post/delayed_job.sh":
    mode: "000755"
    owner: root
    group: root
    content: |
      #!/usr/bin/env bash
      . /opt/elasticbeanstalk/support/envvars
      cd $EB_CONFIG_APP_CURRENT
      su -c "RAILS_ENV=production script/delayed_job --pid-dir=$EB_CONFIG_APP_SUPPORT/pids restart" $EB_CONFIG_APP_USER
```
###For RAILS 4.x
---------------------------
Below  is my delayed_job.sh file for **Rails 4.x**
```sh
commands:
  create_post_dir:
    command: "mkdir /opt/elasticbeanstalk/hooks/appdeploy/post"
    ignoreErrors: true
files:
  "/opt/elasticbeanstalk/hooks/appdeploy/post/delayed_job.sh":
    mode: "000755"
    owner: root
    group: root
    content: |
      #!/usr/bin/env bash
      . /opt/elasticbeanstalk/support/envvars
      cd $EB_CONFIG_APP_CURRENT
      su -c "RAILS_ENV=production bin/delayed_job  --pid-dir=$EB_CONFIG_APP_SUPPORT/pids restart" $EB_CONFIG_APP_USER
```

### What this config do is:

 - Create the "post" directory if it doesn't already exist, it ignores errors (If the directory already exists).
 - Deploy the shell script with the appropriate permission into the right directory.
 

### What this shell script do is:

- Setup the environment variables in (**(/opt/elasticbeanstalk**).
- Restart delayed_job as per the app user (usually **webapp**), using a shared pids directory to make sure it will kill the old version first.