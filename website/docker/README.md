# thingsboard.github.io

Based on [ruby:2.7.7](https://hub.docker.com/_/ruby). For detailed info see origin repository.

## Deployment of the site in docker

The commands below set up the environment for running GitHub pages in the docker. You do not need to install additional dependencies and packages, everything is already built into the docker image.

If you do not have docker installed, you need to install it. You can do this by following the installation instructions: [Docker Engine installation overview](https://docs.docker.com/engine/install/)

Put the site contents and the docker-compose file in the same directory:

```bash
mkdir thingsboard_github_io-docker && cd $_
git clone https://github.com/thingsboard/thingsboard.github.io.git

cat <<EOT | sudo tee docker-compose.yml
version: '3.1'
services:
  thingsboard_website:
    container_name: thingsboard_website
    restart: always
    image: "THINGSBOARD_GITHUB_IO_IMAGE"
    ports:
      - "4000:4000"
    volumes:
      - ./thingsboard.github.io:/website
EOT
```

Execute this command to add webrick for jekyll:
```bash
docker-compose run --no-deps --rm thingsboard_github_io bundle add webrick
```

To start the docker container with docker-compose, run the command:

```bash
docker-compose up
```

In about 2 minutes (depends on PC performance) your copy of the site will be available for viewing at: http://localhost:4000

### Image preview generator in docker

Usage:
```bash
docker exec thingsboard_github_io bash -c "./generate-previews.sh path file_mask*.png"

```

Example:
```bash
docker exec thingsboard_github_io bash -c "./generate-previews.sh images/solution-templates *.png"

```
> **_NOTE:_** This command must be executed with the running container