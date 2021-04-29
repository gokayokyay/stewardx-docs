---
id: frequency
title: Frequency
---

Currently StewardX supports two types of frequencies.

## Hook
Hook frequency works like a webhook. It needs to be triggered, so far it can only be triggered via a http request.

cURL example:
```shell
curl --header "Content-Type: application/json" -X POST http://localhost:3000/execute/#id of your task
```

## Cron
It works like cronjobs but supports **seconds** part. For example following frequency will be executed every 30th minute:

`Every(30 * * * *)`

And this one will be executed every 30th second!

`Every(30 * * * * *)`
