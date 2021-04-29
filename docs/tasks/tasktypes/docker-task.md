---
id: docker-task
title: Docker Tasks
slug: dockertask
---

## Introduction

Docker tasks are handled by the awesome [shiplift crate](https://crates.io/crates/shiplift). To use docker tasks, you'll need to have the docker daemon up and running, and if you use Linux, be sure that the user who runs StewardX can run docker commands without `sudo`. You can find how to add your user to `docker group` [here](https://docs.docker.com/engine/install/linux-postinstall/). A docker task has two subtypes.

## Subtypes

### Docker image

It is the docker image (like the ones you pull from Dockerhub). You provide the `name:tag` and StewardX pulls it then runs it. 

### Dockerfile

To use this type, you provide the contents of the dockerfile. StewardX will try to build it first, then will run it.

## Execution

Whether the subtype is, StewardX will try to pull/build the image and run the container of it. You can provide the environment variables for both subtypes.

## Aborting

To abort a docker task, StewardX will first stops the container, then it kills it 😈.

## Properties

| Property name | Property Type | Definition         |
|---------------|---------------|--------------------|
| image         | Image Object see below | The subtype of task and properties specific to the subtype |
| env           | Array of String | Environment variables of container, item format is "VARNAME=VALUE"|

| Image type    | Type   | Definition                        |
|---------------|--------|-----------------------------------|
| image         | String | It is `image-name:tag`            |
| env           | String | It is contents of the dockerfile. |

## Creating by API

The task creation route is `/tasks`. Sending a `POST` request with required parameters will do.

### `task_props`:

| Property name | Property Type | Definition              |
|---------------|---------------|-------------------------|
| image         | Object        | Image object, see below |
| env           | Array of String | It is the environment array, not required. |

The image property is where you state the subtype of the task. Here's the structure of it:
```json
{
  "t": "It is the type property, either File or Image",
  "c": "It is the content property, so either 'image:tag' or dockerfile contents"
}
```

Other parameters are the same as every task:
- `task_name`: Name of the task (not type)
- `frequency`: Frequency of task, currently `Hook` and `Every(*cron string*)`
- `task_type`: Task type - in this case it's `DockerTask`

cURL example:
```shell
$ curl --header "Content-Type: application/json" -X POST --data '{"task_name": "Hello docker", "frequency": "Hook", "task_type": "DockerTask", "task_props": {"image": { "t": "Image", "c": "hello-world:latest"}}}' http://localhost:3000/tasks
```