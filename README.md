# The Invisible Man Website

## Prequisites
- [Docker](https://www.docker.com/)

## Setup
- Copy `.env.example` to `.env`
- If you're on linux you'll need to add the `WWWGROUP` and `WWWUSER` variables into `.env`. Without setting these values it's likely you'll run into permission errors later on
    - These need to be set to user id and group id of your default user and group. You can find these values by running the `id` command. You're looking for the `uid` (user id)  and `gid` (group id) values 
- Run `docker compose run --no-deps --rm --entrypoint="composer install" web`
- Run `docker compose up -d`
    - This will spin up several containers based on the contents of `docker-compose.yaml`
    - It's possible that the `mysql` container's exposed port will conflict with an existing mysql instance on your host. If that's the case set the env variable `FORWARDED_DB_PORT` to expose a different port on the `mysql` container 
- After the containers are running there are a few commands we'll need to execute inside the `web` container from our `docker-compose.yaml`. There's a few ways to do this
    - You can shell into the container using `docker compose exec web sh`. This will leave you with a prompt in the `web` container that you can use to run each command.
        Note: you'll have to manually exit this shell when you're finished by running the `exit` command
    - You can prefix each command with `docker compose exec web`. This will execute the command in the `web` container and exit when finished.
        - You'll likely be using this prefix a lot, you might want to alias it to something like `dew`
- Run the following commands in the `web` container
    - `php craft install`
        - This command will prompt for admin user credentials. These values will only be used on your local machine. They don't need to be unique or very secure.
    - `php craft migrate/all`
    - `php craft project-config/apply`