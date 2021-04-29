---
sidebar_position: 5
id: API
title: API
---

*Unless otherwise stated, requests' `Content-Type` is `application/json`.*

## Tasks
### Create a task

**Endpoint:** /tasks

**Method:** POST

**Body:**
```json
{
  "task_name": String,
  "frequency": See frequency page,
  "task_type": Currently, it's either DockerTask or CmdTask,
  "task_props": Depends on the task_type
}
```

### Get a task

**Endpoint:** /tasks/:id

**Method:** GET

**Parameters:**

- `id`: ID of the task

### Get all tasks

**Endpoint:** /tasks

**Method:** GET

**Parameters:**

**TODO**: `offset`: Offset

### Delete task

**Endpoint:** /tasks

**Method:** DELETE

**Body:**
```json
{
  "task_id": ID of the task
}
```

**TODO**: Add the RESTful path of deleting the task action.

### Update task

**Endpoint:** /tasks/:id

**Method:** POST

**Parameters:**

- `id`: ID of the task

**Body:**
```json
{
  "task_name": String,
  "frequency": See frequency page,
  "task_type": Currently, it's either DockerTask or CmdTask,
  "task_props": Depends on the task_type
}
```

### Get active tasks

**Endpoint:** /activetasks

**Method:** GET


### Execute a task by body

**Endpoint:** /execute

**Method:** POST

**Body:**

```json
{
  "task_id": ID of the task
}
```

### Execute a task by parameter

**Endpoint:** /execute/:id

**Method:** POST

**Parameters:**

- `id`: ID of the task


### Abort task

**Endpoint:** /abort

**Method:** POST

**Body:**

```json
{
  "task_id": ID of the task
}
```


### Get reports for task

**Endpoint:** /task/:id/reports

**Method:** GET

**Parameters:**

- `id`: ID of the task


### Get latest reports

**Endpoint:** /reports

**Method:** GET


## Get a report

**Endpoint:** /reporst/:id

**Method:** GET
