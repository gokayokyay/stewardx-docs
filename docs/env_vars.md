---
sidebar_position: 3
id: env
title: Environment Variables
---

In order to run StewardX, you'll need some environment variables.

| Variable name         | Description                                                                                                        | Default                            | Required |
|-----------------------|--------------------------------------------------------------------------------------------------------------------|------------------------------------|----------|
| DATABASE_URL          | This variable is only needed **when building StewardX from source.** It is your PostgreSQL database url.         |                                    | false    |
| STEWARDX_DATABASE_URL | This is the variable StewardX looks for. It is the same as DATABASE_URL but required when running the application. |                                    | true     |
| STEWARDX_CONFIG       | The configuration file's path.                                                                                     | $HOME/.config/stewardx/config.json | false    |
| STEWARDX_SERVER_PORT  | Port of the API                                                                                                    | 3000                               | false    |
| STEWARDX_SERVER_HOST  | Host of the API                                                                                                    | 127.0.0.1                          | false    |
