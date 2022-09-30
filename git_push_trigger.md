In this post, I describe how to set up Jenkins and GitHub so that a
build is triggered when a change is pushed to a specific branch.

Note that, in general, a Jenkins job can only have one trigger. So, as
an example, if you want a trigger on pull-request and a trigger when
some changes are pushed to a specific branch, you will need to define
two separate Jenkins jobs.

# Jenkins configuration

On your Jenkins job page:

-   Go to `Configure`.
-   Select the `Source Code Management` tab.
-   In the `Branch to Build` section,
    `Branch Specifier (blank for 'any')`, then enter the
    branch you want to follow, here I am following the branch
    `devel`.
-   in the `Build Triggers` tab, select
    `GitHub hook trigger for GITScm polling`.
-   click `Save`.

Movie:
http://tigress-web.princeton.edu/~luet/jenkins/git_push_trigger_01_1280_720.mp4

# GitHub Configuration

On the GitHub web site:

-   Go to your repository\'s page on GitHub.
-   Select `Settings`, `Webhooks`, `Add webhook`
-   In the Payload URL enter:
    https://jenkins.princeton.edu/github-webhook/
-   In `Content type` select: `application/json`
-   In the box
    `Which events would you like to trigger this webhook?`
    select `Just the push event`.
-   Click on `Add webhook`.
-   Once your webhook has been created, you will get a message on the
    top of your browser window that reads:
    ```
    Okay, that hook was successfully created. We sent a ping payload to
    test it out! Read more about it at
    https://developper.github.com/webhooks/#ping-event.
    ```
-   As you see GitHub sent a ping to the jenkins server. You can check
    that it was successful by clicking on the webhook we just created,
-   Scroll down to the `Recent Deliveries` and look at the
    response, you should have a `200` return code that is
    green, otherwise something went wrong.

Movie:
http://tigress-web.princeton.edu/~luet/jenkins/git_push_trigger_02_1280_720.mp4
