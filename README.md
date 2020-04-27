# A variant of docker-based environment for WP theme development

[*Readme in russian here*](/README.ru.md)

Quick start
---

Just:
 - run `docker-compose up -d`
 - copy any preferable theme boilerplate in to the `./theme` folder
 - go to [localhost:8000](localhost:8000), install WP itself and make any necessary preparations 
 - start rocking!

For those who are not very experienced in using docker and/or wp theme development, a slightly more detailed explanation of the process comes below.   

Prepare environment
---

All stuff here is based on docker and it's infrastructure. You can check it's settings in `docker-compose.yml` file. Consequently, before starting working, you need to have installed these:
- Docker
- Docker-Compose

It is crucial to make sure both of these are working correctly (for instance, there might be not very obvious step with running docker daemon on some systems).

If the command `docker run hello-world` works correctly, then everything else should work too.

Start working
-

The command `docker-compose up` runs the environment. This command is quite convenient while using terminal multiplexer, or you when you just want to see whats happening there (docker prints logs to stdout by default). Moreover, you can stop the whole environment by pressing `Ctrl+C`.

But usually developers use `docker-compose up -d`, which runs containers in detached state. In this case the command will just print several lines of status logs, and then will exit, leaving containers working. In this case you need to run ` docker-compose down` to stop the containers.

After the application has started, you will have two entry points:

- [localhost:8000](localhost:8000) - the website itself
- [localhost:8080](localhost:8080) - PhpMyAdmin

During the first run you will be asked to finish a fresh wordpress installation (to enter administrators credentials). Then all files that belong to wordpress will remain in the `_wp_data`, and usually don't require any additional actions.

If something went wrong, and you need to clean up all created containers and restart everything from scratch, ` docker-compose down --volumes` will clean everything for you.

Project structure
-

- `docker-compose.yml` - main config file, it defines the project infrastructure.
- `_wp_plugins` - directory, which is attached as a volume in place of regular wp-plugins directory. Moved here to the root to make it possible to store all used plugins in the repository. Keep in mind that wordpress keeps the state of plugins in database, not in this directory. So if you've added any new plugins via filesystem, make sure you have enabled them in admin panel, before looking for them on the website. 
- `_wp_uploads` - directory, attached in place of `Uploads` folder. May be useful if you need to store uploaded files in the repository (it is not a good practice though).
- `theme` - theme working folder.

After finishing worpress installation, there should appear two more folders:

- `_wp_data` - local WordPress files. This may be useful in some cases to keep these files explicitly (for IDE autocompletion, of just to be able to collect everything into a complete project archive)
- `_db_data` - database physical files (no special reason, exept to make this docker app completely stateful).

*You may notice the underscore `_` on the start position of some folder names. It was added to make it easier to distinguish important folders that represent certain docker volumes, from other files and folders. In most cases folders that start from underscore don't require any manual intervention*

Project infrastructure
-

After starting docker compose you will get:
- A Mysql database
- Web server with WordPress installed
- PhpMyAdmin on separate port (I think the most common use case to have it in this build - just to be able to import/export database)
