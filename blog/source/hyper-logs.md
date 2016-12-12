title: "Improvements of Container Log"
date: 2016-12-12 22:00:00 +0800
author: hyper
tags:
    - Docker
    - Container
    - Log
    - Hyper.sh
preview: We are excited today to release the new version of container log.

---

We are excited today to release the new version of container log.

In the improved feature, every containers comes with a free 50MB log space (with rotation). The logs are persistent through the container's lifespan. You can access the log messages at anytime with hyper logs (or logs API) unless the container is removed. In the case that container restart (or stop/start), previous logs will still be available, with new messages appended.

We hope the new improvements could help troubleshooting and debugging. Also, we are planning to add the supports for third-party systems, allowing you to forward your logs to external services. Please stay tuned!

Enjoy, The Hyper.sh Crew
