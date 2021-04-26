---
sidebar_position: 2
id: getting-started
title: Getting started
---

# Installing StewardX

Since it is written in Rust, installing StewardX is really easy.

## Precompiled Binary
TODO

## Building it from source
### Prerequisites
- Rust
- git
- Docker (Optional)
- Postgres instance running (Optional if you're willing to use docker)

#### Step 1 - Clone the repository
```shell
git clone https://github.com/gokayokyay/stewardx
cd stewardx
```
Easy isn't it?
#### Optional Step - Disable Docker feature
*You can skip this step if you want to use Docker feature.*

Edit `Cargo.toml` file found on the root of repository, and remove "docker" feature from **default** list, found at the bottom.
```git
[features]
- default = ["docker", "panel", "cmd"]
+ default = ["panel", "cmd"]
```
### Optional Step - Start a Postgresql instance
*You can skip this step if you've a Postgresql instance running, just grab it's URL and scroll to next step.*

StewardX uses Postgres in order to store the task information. It stores:
- Tasks
- Errors
- Execution Reports

You can easily start a database by running a utility script found in `scripts` folder at the root of repository. There're two scripts, actually commands that fires up a database with help of docker. For the sake of simplicity, we'll just use a ephemeral database for this guide.
```shell
./scripts/docker-postgres-temp.sh
```
The URL of the database started by command above is:
```
postgresql://postgres:1234@localhost:5432/postgres
```

### Step 2 - Set the environment variables
Now, in order to compile StewardX, you'll need to set the following environment variables.
```shell
export DATABASE_URL=#Your postgres URL
export STEWARDX_DATABASE_URL=#Your postgres URL
```
If you've done the previous step, in this case those variables will be:
```shell
export DATABASE_URL=postgresql://postgres:1234@localhost:5432/postgres
export STEWARDX_DATABASE_URL=postgresql://postgres:1234@localhost:5432/postgres
```

**NOTE:** DATABASE_URL is only required to build StewardX and it's the same as **STEWARDX_DATABASE_URL**. However, **STEWARDX_DATABASE_URL** variable is required to run StewardX.

### Step 3 - Build it!
You need Rust and Cargo (comes by default if you've followed the instructions on [rustup](https://rustup.rs)). With the variables above set, now run:
```shell
# This may take a while!
cargo build --release
```

If you get any errors, please checkout [troubleshooting section](#troubleshooting) of this guide.

### Step 4 - Start StewardX
When the compilation is finished, you can start StewardX by running:
```shell
./target/release/stewardx
```

If you see nothing, then it's working!

## Troubleshooting

### Building it from source
#### Linker error
If you get the following error:
```shell
error: linker `cc` not found
```
Then just install package(s) below.

Debian/Ubuntu:
```shell
sudo apt install build-essential
```

TODO - Add CentOS/Fedora

#### SSL Error
Install these package(s):

Debian/Ubuntu:
```shell
sudo apt install libssl-dev pkg-config
```

TODO - Add CentOS/Fedora
