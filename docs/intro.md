---
sidebar_position: 1
id: intro
title: Introduction
---

### What is StewardX ?
StewardX stands for **S**cheduled **T**ask **E**xecutor **W**ith **A**synchronous **R**untime and **D**atabase and an **X** on the end. It is a scheduled task executor. Think of cron but with a database. Being written in Rust, gives us a compiled, lightweight single file executable.

### Task types
StewardX currently supports these task types:
- Docker
- Command

But it can be easily extended, thanks to Rust's trait system.

### Rust Features
StewardX consist these features:

| Feature 	| Description 	| Default 	|
|-	|-	|-	|
| docker 	| Enables the `DockerTask` feature, basically docker support. 	| Enabled. 	|
| panel 	| Enables the `panel` feature which is a MVP frontend for StewardX. This feature wraps the `server` feature too. Server feature is basically the API. ***NOTE***: You'll need to add the index.html file path to the config file. 	| Enabled. 	|
| server 	| It is the API. 	| Enabled. 	|
| cmd 	| This feature is for the command line tasks. 	| Enabled. 	|
| server-crud | This feature enables the Create - Read - Update - Delete routes of the server.   | Enabled.  |
