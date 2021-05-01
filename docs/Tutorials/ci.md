---
id: tutorial-ci
title: Creating a CI/CD with CmdTask
---

For this tutorial, we'll create a CI/CD for a React application. In fact, this is how the CI of this documentation made. It's [docusaurus](https://docusaurus.io/), which is essentially a React application. Anyway let's begin!

## Prerequisites
- Up and running instance of StewardX that runs on a server
- Some way to deploy your React application.

For the sake of simplicity, let's assume that React application is server through Nginx.

*To set up Nginx to serve a react app, you can follow the amazing [tutorial on DigitalOcean.](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-react-application-with-nginx-on-ubuntu-20-04)*

## Creating the build script

Create a shell script in some directory.

```shell
cd $HOME
mkdir ci-scripts
cd ci-scripts
touch react-ci.sh
chmod +x react-ci.sh
```

Commands above do the following in order:
- Change current directory to $HOME directory
- Create a directory named `ci-scripts` that'll hold the scripts
- Change current directory to `ci-scripts`
- Create a file named `react-ci.sh`
- Add execute permission to the file

The script will do these things:
- Clone or pull the repository
- Install the dependencies
- Run tests (Skipping this step, since the project just has been created, assuming you've followed the nginx tutorial on digital ocean.)
- If successful issue the build command
- Then deploy the app - will demonstrate for nginx

Okay, first step is to clone or pull the repository. For tha sake of simplicity, the repository in this tutorial is the [documentation repository of StewardX](https://github.com/gokayokyay/stewardx-docs)!

Starting with shebang, then define the variables which will be used.
```shell
#!/bin/bash
PROJECT_DIR_NAME=stewardx-docs
PROJECT_GIT_URL=https://github.com/gokayokyay/stewardx-docs
PROJECT_PARENT=$HOME/projects/
DEPLOY_DIR=/var/www/my-domain.com/html/
```

Now, pulling or cloning the repository
```shell
...
cd $PROJECT_PARENT
if [ -d "$HOME/$PROJECT_DIR_NAME" ] 
then
    echo "Directory ${PROJECT_DIR_NAME} exists. Skipping git clone..." 
    cd $PROJECT_DIR_NAME
    git stash
    git pull
else
    echo "Directory ${PROJECT_DIR_NAME} doesn't exists, cloning it..."
    git clone $PROJECT_GIT_URL
    cd $PROJECT_DIR_NAME
fi
```

Now, installing the dependencies removing the old ones
```shell
...
echo "Cleaning node_modules for a fresh start!"
rm -rf node_modules
echo "Installing the modules..."
npm install
```

Since there are no tests, issuing the build command
```shell
...
echo "Now building it, this can take a while"
npm run build
echo "Cleaning old files in serve directory"
rm -rf $DEPLOY_DIR/*
```

After build is completed, now it's time to move the artifacts to the deploy directory.
```shell
...
echo "Okay, now moving the artifact into the serve directory."
mv build/* $DEPLOY_DIR
echo "Done."
```

After all, your script will look like this:
```shell
#!/bin/bash
PROJECT_DIR_NAME=stewardx-docs
PROJECT_GIT_URL=https://github.com/gokayokyay/stewardx-docs
PROJECT_PARENT=$HOME/projects/
DEPLOY_DIR=/var/www/my-domain.com/html/

cd $PROJECT_PARENT
if [ -d "$HOME/$PROJECT_DIR_NAME" ] 
then
    echo "Directory ${PROJECT_DIR_NAME} exists. Skipping git clone..." 
    cd $PROJECT_DIR_NAME
    git stash
    git pull
else
    echo "Directory ${PROJECT_DIR_NAME} doesn't exists, cloning it..."
    git clone $PROJECT_GIT_URL
    cd $PROJECT_DIR_NAME
fi

echo "Cleaning node_modules for a fresh start!"
rm -rf node_modules
echo "Installing the modules..."
npm install

echo "Now building it, this can take a while"
npm run build
echo "Cleaning old files in serve directory"
rm -rf $DEPLOY_DIR/*

echo "Okay, now moving the artifact into the serve directory."
mv build/* $DEPLOY_DIR
echo "Done."
```

Go ahead and run your script
```shell
./react-ci.sh
```

If it's successfully building and deploying the app, now it's time to create a hook.

## Creating the webhook

Okay, by this time, you have a StewardX instance up and running, a bash script that builds and deploys your app.

### Using Panel

Now, if you've built and used the panel, it's piece of cake for you to add the CmdTask to StewardX.

- Navigate to `Tasks` then `Create a new task` page from sidebar
- Choose `CmdTask`
- Give your task a name
- Choose `Hook` as frequency
- And for the command, enter `/bin/bash $HOME/ci-scripts/react-ci.sh` or point wherever your script is located at

![CI Panel](/img/ci-panel.png)

### Using cURL

If you do not want to use the panel you can run the following command:

```shell
curl --header "Content-Type: application/json" -X POST --data '{"task_name": "React app CI", "frequency": "Hook", "task_type": "CmdTask", "task_props": {"command":"/bin/bash $HOME/ci-scripts/react-ci.sh"}}' http://localhost:3000/tasks
```

Modify `http://localhost:3000/tasks` according to your StewardX host and port. Save the ID in response to use it on GitHub.

## Using the webhook from GitHub

Now, it is time to add the webhook to the GitHub. Navigate to your project's repository, and click settings.

Navigate to `Webhooks` section from the left panel. Click `Add webhook` button found on the top of the page. When the page opens up, you'll want to fill the `Payload URL` with `yourserversurl:STEWARDX_PORT/execute/id_of_your_task`, so it'll look something like `http://mydomain.com:3000/execute/c99ff533-d3c2-4ee3-9b8f-a972a9db00db`.

And congratulations! You've created your own CI!
