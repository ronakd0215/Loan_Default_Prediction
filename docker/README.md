
# Hands-on Machine Learning in Docker

This is the Docker configuration which allows you to run and tweak the book's notebooks without installing any dependencies on your machine!<br/>OK, any except `docker` and `docker-compose`.<br />And optionally `make`.<br />And a few more things if you want GPU support (see below for details).

## Prerequisites

Follow the instructions on [Install Docker](https://docs.docker.com/engine/installation/) and [Install Docker Compose](https://docs.docker.com/compose/install/) for your environment if you haven't got `docker` and `docker-compose` already.

Some general knowledge about `docker` infrastructure might be useful (that's an interesting topic on its own) but is not strictly *required* to just run the notebooks.

## Usage

### Prepare the image (once)

For this, assuming you already downloaded this project into the directory `/path/to/project/loan_default_project`:

```bash
$ cd /path/to/project/loan_default_project/docker
$ docker-compose build
```

This will take quite a while, but is only required once.

After the process is finished you have an `loan_default_project:latest` image, that will be the base for your experiments. You can confirm that by running the following command:

```bash
$ docker images
REPOSITORY            TAG         IMAGE ID            CREATED             SIZE
ageron/handson-ml3    latest      3ebafebc604a        2 minutes ago       4.87GB
```

### Run the notebooks

Still assuming you already downloaded this project into the directory `/path/to/project/loan_default_project`, run the following commands to start the Jupyter server inside the container, which is named `loan_default_project`:

```bash
$ cd /path/to/project/loan_default_project/docker
$ docker-compose up
```

Next, just point your browser to the URL printed on the screen (or go to <http://localhost:8888> if you enabled password authentication inside the `jupyter_notebook_config.py` file, before building the image) and you're ready to play with the book's code!

The server runs in the directory containing the notebooks, and the changes you make from the browser will be persisted there.

You can close the server just by pressing `Ctrl-C` in the terminal window.