# Table of Contents

- [Simple Example](#simple-example)
- [Using Slurm for longer tests](#using-slurm-for-longer-tests)

This document describes how to configure a Jenkins job so that it
is executed periodically (e.g. every 6 hours). The first part goes
through a simple example to show of to configure a scheduled job. This
very simple job is run on the head node. The second part shows how to
use slurm to run on the compute nodes.


# Simple Example

In this example we create a job named `create-date-file` that
will be executed every 1 min. This job will simply create a file
`jenkins-date.dat` that will contains the date of execution.

There is a screen recording here:

<http://tigress-web.princeton.edu/~luet/tutorials/jenkins-schedule/scheduling-jobs-with-Jenkins.mp4>

Here are the steps to follow:
1.  Create a new project. 

	- Select `New Item`, 
	- Give a description: `create-date-file`,
	- Click `Ok`.

2.  Configure.

    a. Select `General` tab:
    
      - Enter a description of your project. I used:

        ``` example
        Create a date file every 1 min
        ```    
    
      - Restrict where this project can be run. I used
    
        ``` example
        adroit_luet
        ```
    
    You should use the name of the agent that the Jenkins
    administrator sent you. Note that there should be no space after the
    name of the agent i.e. in the example if should be "`adroit_luet`"
    not "`adroit_luet​ `". If there is a space, you will get
    the error:
    
    ``` example
    No agent/cloud matches this label expression.
    ```
    
    b. Define what will trigger the execution of your job. Select the
    `Build Triggers` tab.
    
      - Select `Build periodically`. If you click on the `?` next to `Schedule` it will show you an explanation of the expected syntax.
    
      - In this example, since we are debugging, we will run a small task every minutes, so we enter:
    
         ``` example
          * * * * *
         ```
    
       in a production code, it's undesirable to run a tasks that often. 

    c. Define what is going to be executed:
      - Select the `Build` tab
      - Select `Execute shell` to run a shell script.
      - Enter a script to be executed. This example will print the date
        in a file called `jenkins-dat.txt` every minute:
    
       ``` bash
       #!/bin/bash
       date > jenkins-date.txt
       ```
    
    Since we did not specify a directory for the file
    `jenkins-date.txt`, it will be created in the Jenkins' workspace on
    the agent you selected. In this example it's on adroit in:
    
    ``` example
    /home/luet/jenkins/workspace/luet/create-date-file
    ```
    
    d. You can get notifications of the status of the execution:
    
      - Select the `Post-build Actions`.
      - Select `Editable Email Notification`
      - Change the `Project Recipient List`
      - Select `Advanced settings`
      - Remove the default one.
      - Select: `Add Trigger`
      - Pick `Always`.
      - Remove `Developers`. Note that this will send you an email
        every minute. It's good for testing but you should review that
        option quickly after you check the setup. You will get an e-mail
        from `jenkins.rc@princeton.edu`. Once you are done, you can
        either disable or delete the project.

3.  When you are done configuring your job you can test it wit the
    `Build Now` button. This will trigger a build independently of the
    build trigger that you defined earlier. Once the build is complete,
    you can check the logs of the build by clicking on the `Build
    History`.

# Using Slurm for longer tests

Use slurm for longer tests:

``` bash
#!/bin/bash
cd /home/luet/while-loop-srun
salloc -N 1 -n 5 -t 0:5:0 ./while.sh
```

Note that Jenkins will get the status of your job from the `salloc` call.
So if the function `while.sh` returns 0 (success), Jenkins will assume
your job was successful, it will consider the job a failed if the
return code is anything other than 0.
