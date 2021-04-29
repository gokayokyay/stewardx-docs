---
id: cmd-task
title: Command Tasks
slug: cmdtask
---

## Introduction

A primitive task that runs a shell command. However, it needs to be **only one command**. Meaning that you won't be able to do something like this:
```shell
echo "Wait for it..." && echo "-dary."
```

If you want to execute multiple commands, you'd do:
```shell
$ cat << EOF > my-cmd.sh
> echo "Wait for it..." && echo "-dary!"
> EOF
```

That way, you can execute the shell script.
```shell
$ /bin/bash my-cmd.sh
```

## Execution

StewardX will execute the command you've added, so in this case it'll execute `/bin/bash my-cmd.sh`. To not encounter any *path* problems, you'd want to indicate the *absolute path* of the script you created, like:
```shell
/bin/bash /home/stewie/my-cmd.sh
```

## Aborting

StewardX will send the `SIGKILL` signal to the process.

## Properties

| Property name | Property Type | Definition         |
|---------------|---------------|--------------------|
| command       | String        | Command to execute |

## Creating by API

The task creation route is `/tasks`. Sending a `POST` request with required parameters will do.

### `task_props`:

| Property name | Property Type | Definition         |
|---------------|---------------|--------------------|
| command       | String        | Command to execute |

Other parameters are the same as every task:
- `task_name`: Name of the task (not type)
- `frequency`: Frequency of task, currently `Hook` and `Every(*cron string*)`
- `task_type`: Task type - in this case it's `CmdTask`

cURL example:
```shell
$ curl --header "Content-Type: application/json" -X POST --data '{"task_name": "Smile and wave", "frequency": "Hook", "task_type": "CmdTask", "task_props": {"command":"echo hello 👋"}}' http://localhost:3000/tasks
```