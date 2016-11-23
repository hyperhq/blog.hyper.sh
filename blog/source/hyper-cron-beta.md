title: "Hyper Cron (Beta) Release"
date: 2016-11-23 21:40:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Cron
    - Hyper.sh
    - Serverless
preview: We are happy to announce the beta release of Hyper.sh Cron functionality. There was a lot of interest in Hyper Cron from the community and we're looking forward to getting feedback to keep improving the implementation.

---

Today we are happy to announce the beta release of Hyper.sh Cron functionality. There has been a lot of interest in Hyper Cron from the community and we're looking forward to getting feedback to continue improving the implementation.

## How does it work?

As you would expect Hyper Cron is a system service that runs scheduled jobs at given intervals or times, just like the cron unix service.

With Hyper Cron however, you can run any command in a container at given intervals or times and you only pay (per second!) when the cron job is running, just as with any other container on Hyper.sh.

Some members of the community have been calling this **'Serverless Cron'**. The full documentation is linked below, but first let's look at some examples:

### Create a cron job which will ping an address every 5 minutes:

```
$hyper cron create  --minute=*/5 --hour=* --name test-cron-job1 busybox ping -c 3 8.8.8.8
```

### Check cron job list:
```
$ hyper cron ls
Name               Schedule            Image               Command
test-cron-job1     */5 * * * *         busybox             ping -c 3 8.8.8.8
```

### Check cron job execution history:
```
$ hyper cron history test-cron-job1
Container                   Start                           End                             Status              Message
test-cron-job1-1479265800   2016-11-16 03:10:00 +0000 UTC   2016-11-16 03:11:24 +0000 UTC   done                Job[test-cron-job1] is success to run
test-cron-job1-1479266100   2016-11-16 03:15:00 +0000 UTC   2016-11-16 03:15:25 +0000 UTC   done                Job[test-cron-job1] is success to run
test-cron-job1-1479266400   2016-11-16 03:20:00 +0000 UTC   2016-11-16 03:20:25 +0000 UTC   done                Job[test-cron-job1] is success to run
test-cron-job1-1479266700   2016-11-16 03:25:00 +0000 UTC   2016-11-16 03:25:25 +0000 UTC   done                Job[test-cron-job1] is success to run
test-cron-job1-1479267000   2016-11-16 03:30:00 +0000 UTC   -                               -                   Job[test-cron-job1] is running at 2016-11-16 03:30:00 +0000 UTC
```

### Remove a cron job:
```
$ hyper cron rm test-cron-job1
test-cron-job1
```

### Documentation

You can also specify an email for each cron job to receive live status updates. All of the above and more is covered in the documentation [here](https://docs.hyper.sh/Feature/container/cron.html).

### Feedback

We're releasing Hyper Cron as Beta specifically to garner feedback from the community on how this feature should grow. If you have any feedback, you can contact us in any of the following ways.

[Twitter](https://twitter.com/hyper_sh), [Slack](https://slack.hyper.sh/), [Forum](https://forum.hyper.sh/), [Mail](mailto:talk@hyper.sh)

Happy Hacking!

The Hyper.sh Crew
