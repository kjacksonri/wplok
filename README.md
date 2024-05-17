# WPLOK

## DESCRIPTION

This project is meant to createa usable local development environment for WordPress using Docker and Docker Compose.

One of the biggest hurdles with a local Docker development environment is creating a bind mount that can be edited locally, because, by default, the Wordpress image installs everything as root.

We overcome this by creating a Dockerfile that creates a local user within the the Wordpress image.

## INSTALLATION

Do the usual `git clone kjacksonri/wplok {project-name}` to create a new WordPress project.

## EASY CONFIGURATION

If you are just going to work on your WordPress files locally, e.g., create a new Theme or Plugin, and you aren't looking for PHP linting/debugging, etc., then follow these steps:

1. In a terminal, run `id -u`; in the `docker/.env` file, set `MY_UID` to whatever value is returned.
2. In a terminal, run `id -g`; in the `docker/.env` file, set `MY_GID` to whatever value is returned.
3. You should probably change anything in `.env` that has the string `changeme` in it, although everything runs fine with those values.
4. In your terminal, switch to the `docker` directory (`cd docker/`) and run the following command: `docker compose up --detach --build`

To stop your docker containers, in your terminal, switch to the `docker` directory (`cd docker/`) and run the following command: `docker compose down`

To return at a later time, in your terminal, switch to the `docker` directory (`cd docker/`) and run the following command: `docker compose up --detach`

IF YOU CHANGE ANYTHING IN THE `docker` DIRECTORY, YOU NEED TO RUN THIS COMMAND BEFORE CONTINUING TO WORK ON WORDPRESS FILES: `docker compose up --detach --build --force-recreate`
On subsequent runs, you can return to using `docker compose up --detach`

## VSCode configuration

If you are using VSCode as your IDE, you can attach a VSCode to your WordPress container using VSCode's Dev Container extension. However, you need to do some extra configuration the first time you run `docker compose`:

1. In `docker/compose.yml`, comment out line 5 `user: ${MY_UID}:${MY_GID}`.
2. In `docker/.env`, comment out lines 2 (`MY_UID=1000`) and 3 (`MY_GID=1000`)
3. Start the containers: `docker compose up --detach --build`
4. In order to attach to the `wordpress` container, VSCode needs to create a directory in the container underneath `/var/www`. Due to the default permissions in the container, however, VSCode will be unable to do this, so we have to fix the permissions. Run: `docker compose exec wordpress bash -c "chmod 0777 /var/www"`.
   5.Next, look for the "Docker" icon in the vertical left ribbon menu in VSCode (it looks like a whale), an click on it.
5. In the new menu set that appears immediately to the right, you should see "CONTAINERS" with a dropdown arrow just beneath it labeled `docker`. You should see two running containers, one for `mysq` and one for `wordpress`.
6. Right-click on the `wordpress1 container, and click on "Attach Visual Studio Code".
7. A new VSCode window will appear. If you get a error about a terminal, ignore it and dismiss it.
8. In the new VSCode window, press F1 and look for "Dev Containers: Open Container Configuration File". When you find it, click on it.
9. If the file is empty, make it look lke this:

```shell
{
	"workspaceFolder": "/var/www/html",
	"remoteUser": "www-data"
}
```

If there are entries in the file already, add or edit the lines above, as applicable. (If there are lines in the file already, you should probably try to figure out where they came from if you don't know BEFORE you make any changes...)

10. Close the VSCode window.
11. Repeat steps 6 and 7.
12. Begin development.

## VIEWING IN BROWSER

To view your WordPress Site while working on it, go to [http://localhost:8080](http://localhost:8080).

### CAVEAT

This is alpha software, and has not been thoroughly tested, yet. I would appreciate any evaluations, whether positive or negative.
