title: "Improvements of Container Log"
date: 2016-12-12 23:00:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Log
    - Hyper.sh
preview: We are excited today to release the new version of container log feature.

---

Hi All,

We are excited today to release the new version of container log feature.

In the improved feature, every container includes 50MB of free log space (with rotation). The logs are persistent through the container's lifespan. You can access the log messages at anytime with the `hyper logs` (or `logs` API) commands, except if the container had previously been removed. Also, if the container has been restarted (or stop/start), the previous logs will still be available, but with the new log messages appended.

We hope the new improvements will help you in troubleshooting and debugging. Also, we are planning to add support for third-party logging systems, allowing you to forward your logs to an external service. Please stay tuned!

Enjoy,

The Hyper.sh Crew
