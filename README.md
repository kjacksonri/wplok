# WPLOK

## DESCRIPTION

This project is meant to createa usable local development environment for WordPress using Docker and Docker Compose.

One of the biggest hurdles with a local Docker development environment is creating a bind mount that can be edited locally, because, by default, the Wordpress image installs everything as root.

We overcome this by creating a Dockerfile that creates a local user within the the Wordpress image.

### INSTALLATION

Do the usual `git clone kjacksonri/wplok {project-name}`

### CONFIGURATION

All of the configuration in this project is done in the `.env` file. Theoretically, nothing needs to be changed, except possibly the values for `MY_UID` and `MY_GID`.

To get the value for `MY_UID`, run the following in a terminal: `id -u`.

To get the value for `MY_GID`, run the following in a terminal: `id -g`.

You shoud probably change anything in `.env` that has the string `changeme` in it, although everything runs fine with those values.

### CAVEAT

This is alpha software, and has not been thoroughly tested, yet. I would appreciate any evaluations, whether positive or negative.
