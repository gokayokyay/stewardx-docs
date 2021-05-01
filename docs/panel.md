---
sidebar_position: 5
id: panel
title: Panel
---

*Disclaimer: This version of panel is proof of concept and temporary. After StewardX is stabilized, more complete and advanced version of the panel will be built.*

## Introduction
StewardX Panel is the frontend for StewardX. You can add your tasks, edit them or see their execution reports. It's built with React.

## Known limitations
Until the some serving static folder mechanism added to StewardX, it can only serve a single html file as the panel itself. Meaning that the project needs to be bundled into a **single html** file. **Gulp** is used to handle this task.

## Using with StewardX
Since everything is modular and feature based, the panel is a feature based too. It's not shipped with StewardX as default. Meaning that you're free to use any frontend built for StewardX, since this version of panel is currently proof of concept and temporary (not even beta). To use it with StewardX, you need to edit the path in the configuration file.

If you haven't defined a `STEWARDX_CONFIG` environment variable, then it's very likely to StewardX has created it for you. If you've read the [Environment Variables](env_vars.md) page, you should've seen that the default path for the config file is `$HOME/.config/stewardx/config.json`.

Currently the default config file looks like this:

```json
{
  "index_file_path":"index.html",
  "logs_folder_path":"logs"
}
```

As you can see above, there's a `index_file_path` key. This is the key that points to the panel's `index.html` file. If you've modified this file, keep in mind that if you start any of the keys - that ends with `_path` - without a `/` it'll be relative to the config file. For instance, if your config file's path is `/home/mammamia/configs/config.json` then index file's path will be `/home/mammamia/configs/index.html`.

### Getting and building the panel
#### Prerequisites
You'll need to have
- Git
- NodeJS

installed.

#### Clone the repository
Open up your terminal and clone the repository,
```shell
git clone https://github.com/gokayokyay/stewardx-panel
cd stewardx-panel
```

Now install the dependencies with
```shell
npm install
```

If you use yarn then run
```shell
yarn
```

After the installation completed now build it,
```shell
npm run build
# or if you use yarn
yarn build
```

This will create two different directories. Remember that at the time of writing, StewardX will load **only one html file**. Therefore build script has two stages. First it builds the React app (react-scripts) outputting to `build` directory then gulp inlines all of the stuff in build directory to a single html file, outputting to `dist` directory. So the final file will be `stewardx-panel/dist/index.html`.

After the build stage is complete now we can

### Move the index file to the config path

If you changed the index file path in StewardX config, then move the dist file to the path of `index_file_path`.
If you're still using the defaults, open up a terminal and paste the following:
```shell
if [[ -z "${STEWARDX_CONFIG}" ]]; then
  INDEX_DIRECTORY="${HOME}/.config/stewardx/"
else
  INDEX_DIRECTORY="$(dirname $STEWARDX_CONFIG)/"
fi
mkdir -p $INDEX_DIRECTORY
mv dist/index.html $INDEX_DIRECTORY
```

Now, run StewardX and navigate to `https://$STEWARDX_SERVER_HOST:$STEWARDX_SERVER_PORT/app`, the default URL is: `http://localhost:3000/app`.